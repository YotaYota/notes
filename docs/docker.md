
## Dockerfile

First statement is always `FROM`. Determines which base image to use.

## Docker

_volume_: map a folder on your machine accessible from within the Docker image.

```
docker run -v /home/jje/shared-dir:/app alpine
```

`-it` is short for `--interactive` and `--tty`.

`-e` environment variable

```
docker run -e NAME=World node:latest node -e "console.log('Hello ' + process.env.NAME)"
```

`--rm` automatically removes container when it stops.


Run bash inside docker image

```
docker exec -it <image name> bash
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
