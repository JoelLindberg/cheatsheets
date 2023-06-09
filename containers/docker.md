# Docker

For RHEL, I used CentOS Stream 9 for my xperiments: https://docs.docker.com/engine/install/centos/

There is [no RHEL x86_64 support for Docker](https://access.redhat.com/solutions/3696691) except for one of the IBM Z architectures. Red Hat refers to using Podman for RHEL x86_64. As I intend to use Docker Swarm for some experiments then Podman is not possible. [HashiCorp Nomad](https://www.nomadproject.io/) seems to be an interesting alternative to Docker Swarm unless you want to make the jump directly to [K8s](https://kubernetes.io/).

Installation

Source: https://docs.docker.com/engine/install/centos/

## CLI

Shell into a running container: `docker exec -it <name or id> /bin/bash`
View logs of a running container: `docker logs <name or id>`

<br />

## Compose

Installation


Source: https://docs.docker.com/compose/install/linux/#install-using-the-repository

<br />

## Swarm

<br />

## Volumes and Bind mounts

Bind mounts is the easy way to mount a file or folder from the host machine onto a running docker container. This makes the container dependant on a specific structure on the host.

Volumes are stored in Docker's storage directory. They are managed by Docker. This is the preferred way of persisting data.

Sources:

* https://docs.docker.com/storage/bind-mounts/
* https://docs.docker.com/storage/volumes/