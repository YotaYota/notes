
## Dockerfile

First statement is always `FROM`. Determines which base image to use.

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
