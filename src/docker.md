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

To build an image you need to provide _repository name_ and _path_.

Convention for _repository name_ is `<user>/<application>`. If no `<user>/` it means that it is an official image.

Docker client sends the content of the path to the server where it is stored in a working directory called the _build context_.

## Dockerfile

- `FROM` Only required. Determines which base image to use
- `RUN` Execute command
- `ENV` Set environment variable
- `COPY` Copy from build context into image
- `EXPOSE` Expose ports
- `VOLUME` Create directory in image that can be mapped to external storage
- `CMD` Entrypoint when image is _run_

**Note**: All but `CMD` instructions are build during `image build`.

**Note**: Each instruction runs a temporary container and saves a new image for caching.

### Layers

`docker image history <id>` will give steps, `<missing>` means it's a step from the
base image.

## Podman

```
podman run --rm -p 5432:5432 -e "POSTGRES_PASSWORD=postgres" --name pg docker.io/postgres:14
```

## Docker

_volume_: map a folder on your machine accessible from within the Docker image.

```
docker run -v /home/jje/shared-dir:/app alpine
```

`-it` is short for `--interactive` and `--tty`.

`-e` environment variable

`-d` detach

`--rm` automatically removes container when it stops.

```bash
docker run -e NAME=World node:latest node -e "console.log('Hello ' + process.env.NAME)"
```

Run bash inside docker image

```bash
docker exec -it <image name> bash
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
