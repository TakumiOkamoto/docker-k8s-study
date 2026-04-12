# Docker Exercise 01

## Goal

Build the smallest possible custom image and understand what the Dockerfile is actually defining.

## How It Is Structured

- `Dockerfile` defines how an image is built
- `FROM alpine:3.22` selects the base image
- `CMD [...]` defines the default command when a container starts from this image

This exercise is intentionally minimal so the role of each Dockerfile instruction is obvious.

## Build

```bash
docker build -t study-hello .
```

## Run

```bash
docker run --rm study-hello
```

## What This Configuration Means

- `FROM alpine:3.22`
  - The image starts from Alpine Linux version `3.22`
- `CMD ["echo", "hello from docker-k8s-study"]`
  - Unless overridden, the container runs this command at startup
- `-t study-hello`
  - Adds a readable tag to the built image
- `--rm`
  - Removes the container after it exits

## Support Perspective

Questions this exercise should answer:

- Is the problem in the image build stage or the container run stage?
- What command is the container actually executing?
- What changes if `CMD` is overridden during `docker run`?
