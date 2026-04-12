# Kubernetes Basics

## Purpose

This note explains Kubernetes as a control system, not just a tool that runs commands.

## Minimum Concepts

- Cluster
- Node
- Pod
- Deployment
- Service
- Namespace
- ConfigMap
- Secret

## Mental Model

- Docker runs containers.
- Kubernetes manages groups of containers across nodes.
- A Pod is the basic execution unit.
- A Deployment keeps Pods at the desired count.
- A Service gives stable network access.

## Why These Resources Exist

### Pod

The Pod is where the application actually runs. When support work begins, Pod state, events, and logs are usually the first things to inspect.

### Deployment

A Deployment manages Pods declaratively. If a Pod disappears, the Deployment recreates it to match the desired state.

### Service

Pods are replaceable and their IPs are not stable. A Service provides a stable way to reach a set of Pods.

## First Commands

```bash
kubectl get nodes
kubectl get pods -A
kubectl get deployments -A
kubectl get services -A
```

## Basic Apply Cycle

```bash
kubectl apply -f deployment.yaml
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl delete -f deployment.yaml
```

`kubectl apply` means "make the cluster match the desired state described in this file".

## Common Early Mistakes

- Editing Pods directly instead of Deployments
- Forgetting labels and selectors
- Assuming `ContainerCreating` means failure
- Forgetting to expose the Deployment with a Service
