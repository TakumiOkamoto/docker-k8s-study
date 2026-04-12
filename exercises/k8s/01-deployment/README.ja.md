# Kubernetes Exercise 01

## 目的

ローカルクラスタを作り、Deployment と Service を apply して、manifest の各項目が実行時挙動にどう結びつくかを理解する。

## どういう作りになっているか

- `kind` は Docker を使ってローカル Kubernetes クラスタを作る
- `deployment.yaml` には 2 つの resource が入っている
  - nginx Pod を管理する `Deployment`
  - その Pod 群に到達するための `Service`

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

## Manifest の意味

### Deployment

- `replicas: 1`
  - Pod を 1 個維持する
- `selector.matchLabels.app: hello-nginx`
  - この label を持つ Pod をこの Deployment が管理する
- `template.metadata.labels.app: hello-nginx`
  - Deployment が作る Pod 自体にもこの label が付く
- `image: nginx:alpine`
  - 各 Pod は nginx コンテナを実行する
- `containerPort: 80`
  - コンテナが想定している待受ポートを示す

### Service

- `selector.app: hello-nginx`
  - この label を持つ Pod にトラフィックを流す
- `port: 80`
  - クラスタ内で公開する Service のポート
- `targetPort: 80`
  - コンテナの `80` に転送する
- `type: ClusterIP`
  - クラスタ内部向けの到達点にする

## サポート観点

この演習で答えられるようにするべき問い:

- Deployment があっても通信できない時、Service selector と Pod label は一致しているか
- Pod がいても不健康な時、`describe` と `logs` に何が出ているか
- 問題の層は scheduler か、Pod 起動か、コンテナ内部か、Service ルーティングか

## Clean Up

```bash
kubectl delete -f deployment.yaml
kind delete cluster --name study
```
