# 現在の進捗

## 現在地

Docker 演習の進行：
- 演習 01: `hello-world` で基本を確認（完了）
- 演習 02: `nginx:alpine` で bind mount とポート公開を学習（完了）
- 演習 03: Dockerfile を書いて image を build する段階（開始）

現在は 03 を進めるフェーズ。次は build と bind mount の差分を手で確認し、Kubernetes 基礎へ接続する。

## 完了したこと

### Docker 演習

- `exercises/docker/01-hello/` - alpine image で base concept 確認
- `exercises/docker/02-nginx/` - bind mount による既成 image のカスタマイズ
  - ポート公開 (`-p 8080:80`)
  - ファイル mount (`-v`)
  - ホスト、コンテナ、ポート転送の 3 層分離
- **`exercises/docker/03-custom-nginx/`** - 🆕 Dockerfile を書いて build
  - Dockerfile の作成と要素
  - `docker build` による image 生成
  - build vs bind mount の使い分け
  - image と container の関係

### リファレンス・ドキュメント

- `notes/nginx-dockerfile.ja.md / .md`
  - `nginx:alpine` の公式 Dockerfile 解説
  - 各行の役割、daemon off の必要性
  - image サイズ、カスタム Dockerfile テンプレート

## 次にやること（Exercise 03）

1. `exercises/docker/03-custom-nginx` で `docker build -t my-custom-nginx:latest .` を実行する
2. `docker run --rm -p 8080:80 my-custom-nginx:latest` を実行する
3. `localhost:8080` の表示を確認する
4. 演習 02 と比較して「bind mount は即時反映 / build は再ビルド必要」を言葉で説明する
5. 必要なら `docker image inspect my-custom-nginx:latest` で image の状態を確認する

## 完了条件（Exercise 03 開始時点）

- build した image からページが配信されることを確認済み
- 演習 02 と 03 の違いを 3 行で説明できる
- 追加の疑問は `03-custom-nginx` README と `QA.ja.md` に残す
