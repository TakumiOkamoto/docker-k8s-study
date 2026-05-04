# Kubernetes Basics

## Purpose

This note explains Kubernetes as a control plane for containerized workloads, not just a set of commands.

## Why Kubernetes?

Docker Compose is useful for local multi-container setups, but it does not manage the desired state continuously. Kubernetes solves problems that come from:

- application replicas crashing and needing automatic recovery
- changing Pod IP addresses after restart
- routing traffic to dynamic Pod sets
- separating control plane decisions from container runtime operations
- scaling workloads consistently across nodes

## Core concepts

- **Cluster**: one or more nodes managed together
- **Node**: a machine that runs Pods
- **Pod**: the smallest deployable workload unit, one or more containers
- **Deployment**: declarative controller that manages ReplicaSets and Pods
- **Service**: stable network endpoint for a group of Pods
- **Namespace**: logical isolation within a cluster
- **ConfigMap**: non-sensitive configuration data
- **Secret**: sensitive configuration data

## Kubernetes architecture

Kubernetes separates the control plane from the data plane.

### Control plane

- **API server**: the entry point for all requests
- **etcd**: persistent storage for cluster state
- **scheduler**: assigns Pods to nodes
- **controller manager**: enforces desired state for Deployments, Services, etc.

### Data plane

- **kubelet**: agent on each node that manages Pods and containers
- **container runtime**: the software that runs container images
- **kube-proxy**: maintains Service networking on each node

## How Kubernetes works

Kubernetes is declarative:

- You write a manifest describing the desired state
- You apply it with `kubectl apply`
- The control plane compares desired state to actual state
- Controllers take actions to make actual state match desired state

This loop is called reconciliation.

## Key resource types

### Pod

A Pod is the smallest execution unit. It holds one or more containers that share network and storage.

### ReplicaSet / Deployment

A Deployment manages a ReplicaSet, which in turn manages Pods.
The Deployment keeps the requested number of replicas running.

### Service

A Service provides a stable DNS name and IP to route traffic to Pods.
Pods come and go, but the Service endpoint remains constant.

### Namespace

Namespaces segment resources for teams or environments.
They make it easier to scope access and avoid name collisions.

### ConfigMap / Secret

ConfigMaps and Secrets provide configuration to Pods without rebuilding images.
Secrets are intended for sensitive data.

## Local learning with kind

`kind` stands for Kubernetes IN Docker.
It creates Kubernetes node containers on a Docker host.

### Why kind is useful

- fast to create and delete clusters
- uses upstream Kubernetes images
- ideal for learning how a cluster is composed
- does not require a separate VM

### What kind updates in kubeconfig

When you run `kind create cluster --name study`, kind updates `~/.kube/config` with:

- a **cluster** entry: API server endpoint and certificates
- a **user** entry: client credentials
- a **context** entry: a named connection combining cluster and user

The context is usually named `kind-<cluster-name>`.

## Kubernetes distributions

Kubernetes is an API standard and a reference implementation.
Many products and tools package it differently.

### Upstream Kubernetes (`k8s`)

- the source project and API standard
- contains the full set of components and control plane features

### kind

- a local cluster tool that runs upstream Kubernetes in Docker containers
- useful for learning cluster topology and Kubernetes behavior

### k3s

- a lightweight distribution optimized for small or edge environments
- simpler packaging and fewer dependencies
- still supports standard Kubernetes APIs

## First commands

```bash
kubectl get nodes
kubectl get pods -A
kubectl get deployments -A
kubectl get services -A
kubectl get namespaces
kubectl get events -A
```

## Debug flow

1. `kubectl get pods -A`
2. `kubectl describe pod <pod-name>`
3. `kubectl logs <pod-name>`
4. `kubectl get events -A`

## Support checklist

- Is the Pod in the right namespace?
- Do labels and selectors match?
- Is the Deployment replica count correct?
- Is the Service type and port configuration appropriate?
- Are there crashloops or failed scheduling events?

## Common early mistakes

- editing Pods directly instead of Deployments
- forgetting labels and selectors
- assuming `ContainerCreating` means failure
- forgetting to expose a Deployment with a Service
- expecting Pod IPs to stay the same

## Next study steps

- apply `exercises/k8s/01-deployment/deployment.yaml`
- inspect Service routing and Pod labels
- scale the Deployment and observe behavior
- compare `kind` with `k3s` and understand distribution differences
