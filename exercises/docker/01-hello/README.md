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

## Q&A

### Q. What is `alpine`?

A. `alpine` refers to Alpine Linux, a lightweight Linux distribution that is commonly used as a base image in Docker.

Why this exercise uses `alpine:3.22`:

- the image is relatively small
- it is suitable for minimal startup exercises
- it makes the role of `FROM` easy to understand

From a support perspective, remember:

- it does not include the same tools as Debian or Ubuntu based images
- it uses `apk` for package management
- shell and library differences can affect debugging and behavior

### Q. How should `alpine` be pronounced in Japanese?

A. In Japanese conversation, `Arupain` (`アルパイン`) is the natural pronunciation and will be understood in Docker discussions.

Notes:

- using a close English-style pronunciation is enough
- `Alpine Linux` is commonly said as `アルパイン Linux`
- it is not usually pronounced like `アルピネ`
