# Docker

Contains:

- Server
- Docker client CLI
- REST API for client server communication

## Container

_Containers_ are run from an _image_.

Container types are

- short-running tasks that exits when they are done
- long-running jobs that are detached
- interactive that stays alive while Docker client is connected

## Image

`docker build` builds Docker images from a Dockerfile and a build context.

*Build context*: the set of files that your build can access.

To build an image you need to provide _repository name_ and _path_, and optionally _tag_ which defaults to "latest".

Convention for _repository name_ is `<user>/<application>`. If no `<user>/` it means that it is an official image. _Tag_ is usually used for version.

`<user>/<application>:<tag>`

Docker client sends the content of the path to the server where it is stored in a working directory called the _build context_.

## Dockerfile

- `FROM` Only required. Determines which base image to use
- `RUN` Execute command
- `ENV` Set environment variable
- `COPY` Copy from build context into image (`ADD` is an extended version of `COPY`)
- `EXPOSE` Expose ports
- `VOLUME` Create directory in image that can be mapped to external storage
- `CMD` Entrypoint when image is _run_

**Note**: All but `CMD` instructions are build during `image build`.

**Note**: Each instruction runs a temporary container and saves a new image for caching.

### Layers

`docker image history <id>` will give steps, `<missing>` means it's a step from the base image.

## Volume

Docker images can only be changed by explicitly building a new one. To be able to persist data that should be available for another container run from that image, you must use _volumes_.

_volume_: map a folder on host machine (or other storage) accessible from within the Docker image.

Volumes are owned by one container but can be shared with others.

## Docker Registry

Docker Hub is the default registry.

To run a local private registry

```bash
docker container run -d -p5000:5000 registry:latest
```

To push an image to it you need to tag it with the address, and the push it

```bash
docker image tag <user>/<app> localhost:5000/<user>/<app>
docker image push localhost:5000/<user>/<app>
```

list the contents with

```bash
curl http://localhost:5000/v2/_catalog
```

## Docker Compose

Used for running multiple containers as a single service.

## Docker Swarm

Service for controlling multiple docker environments within a single platform.

Each node is Docker daemon, and all Docker daemons interact using the Docker API.

2 types of nodes:

- Manager node
- Worker node

## Commands

`systemctl start Docker`

`docker rmi image_id`

`docker image history <image>`

`docker pull image_name`

`docker run <image_id>`

`docker build -t [image_name]:tag`

`-it` is short for `--interactive` and `--tty`.

`-e` environment variable

`-d` detach

`--rm` automatically removes container when it stops.

Run bash inside docker image

```bash
docker exec -it <image name> bash
```

## Podman

```
podman run --rm -p 5432:5432 -e "POSTGRES_PASSWORD=postgres" --name pg docker.io/postgres:14
```

## Logs

```bash
podman logs --follow <CONTAINER ID>
```

## Remove

Clean up all dangling resources:

```bash
docker system prune
```

Also remove any stopped containers and all unused images

```bash
docker system prune -a
```
