# Setup

## Install

### Core

```bash
brew install --cask docker
brew install kubectl kind
```

Optional:

```bash
brew install k9s helm
```

## Verify

```bash
docker --version
kubectl version --client
kind version
```

## Docker Desktop

1. Open Docker Desktop once after install.
2. Wait until Docker engine is ready.
3. Confirm:

```bash
docker run hello-world
```

## What To Learn First

- Images
- Containers
- Ports
- Volumes
- Logs
- Dockerfile
- Compose

## Avoid Early Confusion

- Docker image != running container
- `EXPOSE` does not publish a port by itself
- Kubernetes is not the first thing to learn
