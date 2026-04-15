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

## 次にやること（Kubernetes 基礎）

1. `notes/10-k8s-basics.ja.md` を読み、Kubernetes の全体像と用語を把握する
2. k8s, k3s, kind などの「K8sディストリビューション」の違いを理解する
3. `kubectl` と `kind` のバージョンやインストール状態を確認する

## 完了条件（Kubernetes 基礎開始時点）

- K8sが解決したい課題（なぜ Docker Compose だけではダメなのか）を理解できる
- `kind` を使ってローカルクラスタを構築できる
