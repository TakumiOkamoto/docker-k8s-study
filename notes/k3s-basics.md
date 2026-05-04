# k3s Basics

## Purpose

This note explains what k3s is, why it exists, and how it compares with upstream Kubernetes and other lightweight local cluster tools.

## What is k3s?

k3s is a lightweight Kubernetes distribution created by Rancher. It aims to make Kubernetes easier to run in resource-constrained environments, edge devices, and simple local labs.

## Why k3s exists

- Kubernetes upstream contains many components, plugins, and optional providers.
- k3s reduces operational complexity by packaging a smaller set of defaults into one binary.
- It is designed to start quickly and run on low-memory systems.

## Core differences from upstream Kubernetes

- Single binary: `k3s` includes server and agent components in one distribution.
- Embedded datastore: k3s can use SQLite by default instead of etcd.
- Built-in container runtime: containerd is bundled and configured by default.
- Optional addons: Traefik, servicelb, and metrics-server can be enabled automatically.
- Fewer dependencies: no need for separate etcd, CNI, or external load balancer by default.

## How k3s is still Kubernetes

- k3s supports the Kubernetes API and resource model.
- Standard manifests such as Deployment, Service, ConfigMap, and Secret work the same way.
- `kubectl` can be used against a k3s cluster.
- The difference is mostly in packaging, defaults, and operational footprint, not in the basic API semantics.

## Typical use cases

- Development and learning
- CI/CD pipelines and test clusters
- Edge computing and IoT devices
- Small lab clusters
- Environments where a full upstream Kubernetes control plane is too heavy

## Installation and runtime

### Simple install

```bash
curl -sfL https://get.k3s.io | sh -
```

### Verify installation

```bash
sudo k3s kubectl get nodes
sudo k3s kubectl get pods -A
```

### Alternative: k3d for Docker-based k3s clusters

- `k3d` is a helper tool that launches k3s clusters inside Docker containers.
- It is useful for quick experimentation on a workstation.

## How it behaves differently from kind

- `kind` builds a Kubernetes cluster using upstream node images and Docker containers.
- `k3s` is a packaged Kubernetes distribution; it can run directly on a Linux host or inside Docker via `k3d`.
- `kind` is ideal for learning how upstream cluster nodes are created with container images.
- `k3s` is ideal for learning a lightweight Kubernetes distribution with simpler startup.

## Support engineer view

- When supporting k3s, remember that the core API is still Kubernetes.
- Differences are often in defaults and internal components, not in basic manifest behavior.
- Logs and troubleshooting usually start with:
  - `kubectl get pods -A`
  - `kubectl describe pod <pod>`
  - `kubectl logs <pod>`
  - `sudo journalctl -u k3s` (on the host)
- Watch for distribution-specific services such as `traefik` and `local-path-provisioner`.

## Troubleshooting hints

- If the cluster does not start, check the k3s server logs.
- If pods stay pending, check node status and available resources.
- If Service routing fails, check the Service type and selector labels.
- If storage fails, understand that k3s may use a local path provisioner by default.

## Relation to Kubernetes distributions

- Upstream Kubernetes is the source project and API standard.
- k3s is one distribution built for lightweight deployments.
- kind is another tool/distribution optimized for local Kubernetes clusters in Docker.
- In support work, the key is to identify whether an issue is related to:
  - API/object configuration
  - node or runtime environment
  - distribution-specific defaults
