# 現在の進捗

## 現在地

Docker の初期セットアップは完了。最初の Dockerfile 演習も完了。次は `nginx` を使って、ポート公開と bind mount を体験し、ホスト側、コンテナ側、ネットワーク公開のどこに責務があるかを整理する段階。

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

1. `exercises/docker/02-nginx` で `docker run --rm -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine` を実行する
2. `localhost:8080` で `index.html` が見えることを確認する
3. ホスト、コンテナ、ポート公開の 3 層に分けて説明できるようにする
4. `kind` で最初のローカルクラスタを作る
5. Kubernetes の Deployment と Service を apply する

## 今の理解ポイント

- Docker Engine は Docker Desktop 起動後に利用できる
- `docker build` でイメージを作り、`docker run` でコンテナを実行する
- `kubectl` と `kind` はすでに利用可能

## 直近の学習テーマ

- Docker のイメージとコンテナの違い
- ポート公開
- bind mount
- Kubernetes の Pod / Deployment / Service
- 設定ファイルが何を制御しているか
- トラブル時にどの層を見るべきか

## 今回の再開メモ

- `exercises/docker/02-nginx/README.ja.md` に実行前チェック、実行後確認、切り分け順を追加済み
- この sandbox からは Docker ソケットに接続できず、`docker info` の再確認は未実施
- 実機では利用者が `02-nginx` のコマンドを実行すれば次に進める
