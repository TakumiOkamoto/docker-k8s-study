# Kubernetes Exercise 01

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

## Clean Up

```bash
kubectl delete -f deployment.yaml
kind delete cluster --name study
```
