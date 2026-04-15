# 現在の進捗

## 現在地

Docker 演習の進行：
- 演習 01: `hello-world` で基本を確認（完了）
- 演習 02: `nginx:alpine` で bind mount とポート公開を学習（完了）
- 演習 03: Dockerfile を書いて image を build する段階（完了）
- 演習 04: Docker Compose で複数コンテナを管理する（開始）

現在は 04 を進めるフェーズ。Docker Compose を使ったインフラのコード化（IaC）の基礎を学ぶ。

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
- **`exercises/docker/04-compose/`** - 🆕 Docker Compose の基礎
  - `compose.yaml` の構造と意味

### リファレンス・ドキュメント

- `notes/nginx-dockerfile.ja.md / .md`
  - `nginx:alpine` の公式 Dockerfile 解説
  - 各行の役割、daemon off の必要性
  - image サイズ、カスタム Dockerfile テンプレート

## 次にやること（Exercise 04）

1. `exercises/docker/04-compose` ディレクトリを作成する
2. はじめての `compose.yaml` を作成する
3. `docker compose up` での起動を体験し、なぜこれが便利なのか理解する

## 完了条件（Exercise 04 開始時点）

- `docker run` と `docker compose` の違い・関係性を理解できる
- 追加の疑問は `04-compose` README と `QA.ja.md` に残す
