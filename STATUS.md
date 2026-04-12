# Current Status

## Current Stage

Docker setup is complete. The first Dockerfile exercise is complete. The next step is to use nginx to understand port publishing and bind mounts, and to separate host-side, container-side, and published-network responsibilities.

## Completed

- Created the private study repository for Docker and Kubernetes
- Created the initial Git commit
- Configured the GitHub `origin`
- Opened Docker Desktop
- Verified `docker info`
- Ran `docker run hello-world`
- Verified `kubectl` installation
- Verified `kind` installation
- Completed the first Dockerfile exercise
  - `docker build -t study-hello .`
  - `docker run --rm study-hello`

## Available Tools

- Docker Desktop
- Docker CLI
- `kubectl`
- `kind`

## Next Steps

1. Run `exercises/docker/02-nginx`
2. Understand port publishing and bind mounts
3. Create the first local Kubernetes cluster with `kind`
4. Apply a Kubernetes Deployment and Service

## Current Understanding Targets

- the difference between Docker images and containers
- port publishing
- bind mounts
- Kubernetes Pod, Deployment, and Service
- what each configuration file controls
- which layer to inspect during troubleshooting
