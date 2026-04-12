# Setup

## Purpose

This note explains not only how to install the tools, but also what role each tool plays in the local Docker and Kubernetes workflow.

## Components

### Docker Desktop

On macOS, Docker Desktop provides the runtime environment needed to run Linux containers. It is more than the `docker` command. It includes the Docker Engine and the supporting components required to run containers locally.

### Docker CLI

The `docker` command is the client. It sends requests to the Docker daemon. If the daemon is not running, the CLI alone is not enough.

### kubectl

`kubectl` is the Kubernetes client. It talks to the Kubernetes API server and is used to inspect or apply cluster resources.

### kind

`kind` stands for Kubernetes in Docker. It creates a local Kubernetes cluster using Docker containers as nodes.

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

What these checks mean:

- `docker --version`: the Docker CLI is installed
- `kubectl version --client`: the Kubernetes client is installed
- `kind version`: the local cluster tool is installed

## Why Docker Desktop Must Be Opened Once

On macOS, having the Docker CLI installed does not mean the container runtime is available. Docker Desktop starts the Docker Engine that actually runs containers.

## First Connectivity Check

```bash
docker run hello-world
```

This confirms:

- the CLI can reach the daemon
- an image can be pulled
- a container can start
- the container output can be returned to the terminal

## Common Early Confusions

- A Docker image is not the same as a running container
- The Docker CLI is not the same as the Docker Engine
- Having `kubectl` installed does not mean a cluster exists
- Having `kind` installed does not mean a cluster is running
- `EXPOSE` does not publish a port by itself
