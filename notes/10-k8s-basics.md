# Kubernetes Basics

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

## Common Early Mistakes

- Editing Pods directly instead of Deployments
- Forgetting labels and selectors
- Assuming `ContainerCreating` means failure
- Forgetting to expose the Deployment with a Service
