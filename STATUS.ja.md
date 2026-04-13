# 現在の進捗

## 現在地

Docker 演習の進行：
- 演習 01: `hello-world` で基本を確認
- 演習 02: `nginx:alpine` で bind mount とポート公開を学習
- 演習 03: **新規** Dockerfile を書いて image を build する方法を習得

次は、build と bind mount の使い分け、そして Kubernetes の基礎に進む段階。

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
