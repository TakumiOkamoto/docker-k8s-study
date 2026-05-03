# 現在の進捗

## 現在地

Docker 演習の進行：
- 演習 01: `hello-world` で基本を確認（完了）
- 演習 02: `nginx:alpine` で bind mount とポート公開を学習（完了）
- 演習 03: Dockerfile を書いて image を build する段階（完了）
- 演習 04: Docker Compose で複数コンテナを管理する（完了）
- フェーズ移行: Kubernetes 基礎へ（開始）

現在は Kubernetes の基礎（Day 5相当）へ進むフェーズ。Docker で学んだ構造をベースに Kubernetes エコシステムへ接続する。

## 完了したこと

### Docker 演習

- `exercises/docker/01-hello/` - alpine image で base concept 確認
- `exercises/docker/02-nginx/` - bind mount による既成 image のカスタマイズ
  - ポート公開 (`-p 8080:80`)
  - ファイル mount (`-v`)
  - ホスト、コンテナ、ポート転送の 3 層分離
- `exercises/docker/03-custom-nginx/` - Dockerfile を書いて build
  - Dockerfile の作成と要素
  - `docker build` による image 生成
  - build vs bind mount の使い分け
  - image と container の関係
- `exercises/docker/04-compose/` - Docker Compose の基礎 (完了)
  - `compose.yaml` の構造と意味

### リファレンス・ドキュメント

- `notes/nginx-dockerfile.ja.md / .md`
  - `nginx:alpine` の公式 Dockerfile 解説
  - 各行の役割、daemon off の必要性
  - image サイズ、カスタム Dockerfile テンプレート

## 完了した（Kubernetes 基礎フェーズ）

- `notes/10-k8s-basics.ja.md` を読み、用語と心構えを把握
- K8s ディストリビューション（`k8s`, `k3s`, `kind`）の違いを理解
- `kind create cluster --name study` でローカルクラスタを構築
- `exercises/k8s/01-deployment/deployment.yaml` を apply
- Pod 作成と Service 設定を確認
- ログ体系を探索: `kubectl logs`, `kubectl describe pod`
- 3層のログ（app logs / Pod event / node OS log）を理解
- kubeconfig 構造（cluster, user, context）を学習

## 現在の理解度

- Deployment は Pod 複製数を宣言的に管理する
- Service は Pod グループへの stable ClusterIP を提供する
- Pod 名はハッシュサフィックス（ReplicaSet ID + ランダム）で自動生成
- ログはコンテナの stdout/stderr であり、ファイルではない
- Pod イベントはライフサイクルと状態変化を表示する

## 次にやること（Kubernetes 学習継続）

1. Pod 削除と Deployment による自動再作成を実験
2. 別 Pod からの Service ネットワーキングをテスト
3. Deployment レプリカをスケール、動作を観察
4. 複数 Deployment + Service でルーティングを学習
5. 複数 namespace 演習へ進む
