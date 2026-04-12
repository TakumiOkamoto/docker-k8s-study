# 現在の進捗

## 現在地

Docker の初期セットアップは完了。最初の Dockerfile 演習も完了。次は `nginx` を使って、ポート公開と bind mount を体験する段階。

## 完了したこと

- Docker + Kubernetes 学習用の private repository を作成
- Git の初回コミットを作成
- GitHub の `origin` を設定
- Docker Desktop を起動
- `docker info` を確認
- `docker run hello-world` を実行
- `kubectl` の導入を確認
- `kind` の導入を確認
- 最初の Dockerfile 演習を完了
  - `docker build -t study-hello .`
  - `docker run --rm study-hello`

## 現在使えるもの

- Docker Desktop
- Docker CLI
- `kubectl`
- `kind`

## 次にやること

1. `exercises/docker/02-nginx` を実行する
2. ポート公開と bind mount を理解する
3. `kind` で最初のローカルクラスタを作る
4. Kubernetes の Deployment と Service を apply する

## 今の理解ポイント

- Docker Engine は Docker Desktop 起動後に利用できる
- `docker build` でイメージを作り、`docker run` でコンテナを実行する
- `kubectl` と `kind` はすでに利用可能

## 直近の学習テーマ

- Docker のイメージとコンテナの違い
- ポート公開
- bind mount
- Kubernetes の Pod / Deployment / Service
