# Podman

Official sites:
* https://podman.io
* https://podman-desktop.io/

## Install on Ubuntu 22.04

~~~console
sudo apt install podman
~~~

Podman seems to be a great alternative to Docker, especially on the development side since it's not running as a daemon. It seems to be a kit of applications which all provide the same cli utilities as docker. You can even build your images from a Dockerfile with on modifications.

It supports multiple container engines:
* Podman
* Docker
* Lima

Source: \
https://developers.redhat.com/blog/2019/02/21/podman-and-buildah-for-docker-users#how_does_docker_work_

## Managing containers using Podman

Source: \
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_atomic_host/7/html/managing_containers/finding_running_and_building_containers_with_podman_skopeo_and_buildah

## Pull containers from Docker

You need to prefix the registry name with docker.io: `docker.io/<registry name>`

**pgadmin4**

To pull for example: https://hub.docker.com/r/dpage/pgadmin4, specify the following as image: `docker.io/dpage/pgadmin4`

Required variables: \
PGADMIN_DEFAULT_EMAIL \
PGADMIN_DEFAULT_PASSWORD

**postgresql**

To get official images such as https://hub.docker.com/_/postgres, add /library, for example postgresql official: `docker.io/library/postgres`

Required variables: \
POSTGRES_PASSWORD

Source: \
https://stackoverflow.com/questions/69162077/podman-pull-official-images-from-docker-hub
