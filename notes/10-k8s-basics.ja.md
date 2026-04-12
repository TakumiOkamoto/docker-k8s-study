# Kubernetes 基礎

## この章の目的

Kubernetes を「便利な実行コマンド群」としてではなく、「望ましい状態を維持するための制御面」として理解する。

## 最低限の構成要素

- Cluster
- Node
- Pod
- Deployment
- Service
- Namespace
- ConfigMap
- Secret

## どういう作りになっているか

- Docker はコンテナを実行する
- Kubernetes は複数の実行単位を、宣言された状態に合わせて管理する
- Pod はコンテナの実行単位
- Deployment は Pod の数や更新方針を管理する
- Service は Pod 群への安定したアクセス方法を提供する

## サポート観点で重要な見方

### Pod

アプリケーションが実際に動いている最小単位。問題発生時はまず Pod の状態、イベント、ログを見る。

### Deployment

Pod を直接作るのではなく、Deployment が Pod を管理する。Pod が落ちた時に再作成されるのは Deployment などの controller がいるから。

### Service

Pod は入れ替わる可能性があるため、直接 Pod IP に依存しない。Service が安定した到達先になる。

## 最初に見るコマンド

```bash
kubectl get nodes
kubectl get pods -A
kubectl get deployments -A
kubectl get services -A
```

## apply の意味

```bash
kubectl apply -f deployment.yaml
```

このコマンドは「この manifest に書かれた望ましい状態をクラスタに反映する」という意味。単にファイルを読むだけではない。

## トラブル時の基本観察

```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

- `describe`
  - スケジューリング、イベント、失敗理由を見る
- `logs`
  - アプリケーション出力を見る

## 初学者が誤解しやすい点

- Pod を直接作って終わりにしがち
- `selector` と `labels` の対応を軽視しがち
- `ContainerCreating` を即失敗と誤解しがち
- Service がないのに外部到達性を期待しがち
