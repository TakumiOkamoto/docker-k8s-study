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

1. Run `docker run --rm -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine` in `exercises/docker/02-nginx`
2. Confirm that `index.html` is visible at `localhost:8080`
3. Be able to explain the host, container, and port-publishing layers separately
4. Create the first local Kubernetes cluster with `kind`
5. Apply a Kubernetes Deployment and Service

## Current Understanding Targets

- the difference between Docker images and containers
- port publishing
- bind mounts
- Kubernetes Pod, Deployment, and Service
- what each configuration file controls
- which layer to inspect during troubleshooting

## Resume Note

- `exercises/docker/02-nginx/README.md` now includes pre-run checks, post-run verification, and a troubleshooting order
- Docker socket access is blocked from this sandbox, so live `docker info` re-verification was not possible here
- The concrete next action on the user's machine is to run the `02-nginx` command locally
