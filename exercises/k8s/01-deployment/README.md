# Kubernetes Exercise 01

## Goal

Create a local cluster, apply a Deployment and a Service, and understand how the manifest maps to runtime behavior.

## How It Is Structured

- `kind` creates a local Kubernetes cluster using Docker
- `deployment.yaml` defines two resources
  - a `Deployment` that manages nginx Pods
  - a `Service` that targets those Pods

## Create A Cluster

```bash
kind create cluster --name study
```

## Apply

```bash
kubectl apply -f deployment.yaml
kubectl get pods
kubectl get services
```

## Inspect

```bash
kubectl describe deployment hello-nginx
kubectl logs deployment/hello-nginx
```

## What The Manifest Means

### Deployment

- `replicas: 1`
  - Keep one Pod running
- `selector.matchLabels.app: hello-nginx`
  - The Deployment manages Pods with this label
- `template.metadata.labels.app: hello-nginx`
  - The Pods created by this Deployment get this label
- `image: nginx:alpine`
  - Each Pod runs the nginx container image
- `containerPort: 80`
  - Documents the intended listening port for the container

### Service

- `selector.app: hello-nginx`
  - Route traffic to Pods with this label
- `port: 80`
  - Service port exposed inside the cluster
- `targetPort: 80`
  - Forward traffic to container port `80`
- `type: ClusterIP`
  - Make the Service reachable inside the cluster only

## Support Perspective

Questions this exercise should answer:

- If the Deployment exists but traffic fails, does the Service selector match the Pod labels?
- If the Pod exists but is unhealthy, what do `describe` and `logs` say?
- Which layer is failing: scheduler, Pod startup, container process, or Service routing?

## Clean Up

```bash
kubectl delete -f deployment.yaml
kind delete cluster --name study
```
