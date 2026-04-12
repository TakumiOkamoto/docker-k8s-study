# Docker Exercise 02

## 目的

ローカルの HTML ファイルを nginx コンテナから配信し、ポート公開と bind mount を理解する。

## どういう作りになっているか

- `nginx:alpine` は既成の Web サーバイメージ
- `index.html` はホスト側のローカルファイル
- `-v` でそのファイルをコンテナ内に見せる
- `-p 8080:80` でホストのポートとコンテナのポートを結びつける

## Run

```bash
docker run --rm -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine
```

ブラウザで <http://localhost:8080> を開く。

## この設定の意味

- `nginx:alpine`
  - 自前ビルドせず、公開済みの nginx イメージを使う
- `-p 8080:80`
  - ホストの `8080` をコンテナの `80` に転送する
- `-v "$PWD/index.html:/usr/share/nginx/html/index.html:ro"`
  - ローカルの `index.html` をコンテナ内の nginx 配信ファイルとして差し込む
  - `:ro` は読み取り専用
- `--rm`
  - 終了時にコンテナを自動削除する

## サポート観点

この演習で答えられるようにするべき問い:

- 問題はホスト側か、コンテナ側か、ポート転送か
- 期待した HTML が出ない時、mount 先のパスは正しいか
- ページが開かない時、nginx は起動しているか、ポート公開は正しいか
