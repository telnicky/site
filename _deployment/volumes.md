---
title: "Volumes"
order: 600
---

You can mount volumes from your host into your application Processes.

## Shared Filesystem

Convox uses a network filesystem backed by EFS that is shared among all of the instances in your Rack. You can use this in conjunction with [Docker host volume mounting](https://docs.docker.com/v1.11/engine/userguide/containers/dockervolumes/#mount-a-host-directory-as-a-data-volume) to persist files in your Processes across restarts and to share files between Processes.

## Sharing Data

You can mount a volume by specifying the container path. In this case, Convox will mount this directory on a network filesystem to be shared between Processes of the same type.

```
web:
  volumes:
    - /my/container/path
```

<div class="block-callout block-show-callout type-info" markdown="1">
If you'd like to specify the different host path as well you can do so in the `host:container` format, e.g. `/foo:/my/container/path`.
</div>

## Example: WordPress

[WordPress](https://wordpress.com/) is a popular PHP blogging platform. It expects a persistent filesystem for storing themes, plugins, and media uploads. Rack Processes are stateless and disposable by default, but you can persist files using volumes as shown in the following example docker-compose.yml`.

```
web:
  image: wordpress:4.5.2-apache
  ports:
    - 80:80
  volumes:
    - /var/www/html
```

This configuration will work with both `convox start` and `convox deploy`. Any files that are written to `/var/www/html` in the container will be persisted and shared.