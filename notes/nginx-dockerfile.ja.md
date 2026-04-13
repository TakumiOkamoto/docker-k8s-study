# nginx:alpine の Dockerfile

## 公式 Dockerfile の構造

`nginx:alpine` イメージの Dockerfile は非常にシンプルです。

```dockerfile
FROM alpine:latest

RUN apk add --no-cache nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

参考：
- 公式イメージ: https://hub.docker.com/_/nginx
- 公式リポジトリ: https://github.com/nginxinc/docker-nginx

## 各行の説明

### `FROM alpine:latest`

- ベースイメージとして `alpine:latest` を使用
- Alpine Linux は軽量な Linux ディストリビューション
- Docker イメージサイズが小さくなる（約 8-10MB）

### `RUN apk add --no-cache nginx`

- `apk` = Alpine Linux のパッケージマネージャー（apt や yum のようなもの）
- `add` = パッケージを追加インストール
- `--no-cache` = インストール後のキャッシュを削除してイメージサイズをさらに削減
- つまり、パッケージマネージャーの キャッシュメタデータは image に含めない

### `EXPOSE 80`

- コンテナが port 80 でリッスンすることを「宣言」する
- この行は docker run の時に `-p` を指定することを示唆
- 注意：`EXPOSE` だけでホスト側に公開されるわけではなく、`docker run -p` で初めて公開される

### `CMD ["nginx", "-g", "daemon off;"]`

- コンテナ起動時に実行するデフォルトコマンド
- `nginx` = nginx サーバプロセスを起動
- `-g "daemon off;"` = nginx をバックグラウンドデーモンではなくフォアグラウンドで実行する
- 理由：コンテナは、メインプロセス(PID 1)が終了したら全体が終了する。デーモンモードは background で動くため、コンテナが終了してしまうから

## デフォルト設定

`nginx:alpine` は上記の Dockerfile だけで、次の設定も既に含まれています：

- nginx のデフォルト設定ファイル: `/etc/nginx/nginx.conf`
- 配信ファイルの置き場所: `/usr/share/nginx/html`
- リッスンポート: `80`
- ユーザー: `nginx`

## この演習との関連

### 既成 image をそのまま使う場合（演習 02）

```bash
docker run --rm -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine
```

- `nginx:alpine` の Dockerfile はそのまま
- 配信ファイルだけを bind mount で差し替える

### 独自の image を build する場合

```dockerfile
# 自分の Dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
COPY css/ /usr/share/nginx/html/css/
```

- `nginx:alpine` をベースにして
- `COPY` で自分のファイルをコンテナに追加
- その結果、新しい image が出来上がる

このように、`FROM nginx:alpine` で公式 image を extend して、自分のコンテンツを追加するのが標準的な Dockerfile の書き方。

## イメージサイズの比較

| Image | サイズ | 理由 |
|-------|--------|------|
| `nginx:alpine` | 約 50-100 MB | 軽量 Linux + nginx 最小構成 |
| `nginx:latest` (Debian ベース) | 約 180-250 MB | Debian + 余計なツール多め |
| `nginx:1.27-alpine` | 約 50-100 MB | 特定バージョン + 軽量 |

Alpine ベースは本当に軽量。

## 補足：`daemon off;` の重要性

通常の Linux では、nginx は `daemon on;` で background で動きます。

```bash
# 通常の Linux
systemctl start nginx
# バックグラウンドで ng inx が起動
```

しかしコンテナは異なる。コンテナの主プロセス(PID 1)が終了すると、コンテナ全体が終了します：

```
CMD ["nginx", "-g", "daemon off;"]
^
このコマンドが PID 1 になる

もし daemon on; だと...
→ nginx がバックグラウンドに移行
→ CMD は完了して終了
→ PID 1 がないのでコンテナが終了してしまう（困る）
```

そのため、コンテナ用の nginx は常に `daemon off;` で foreground 実行する設計になっています。

## 参考：独自 Dockerfile で build する時のテンプレート

```dockerfile
FROM nginx:alpine

# 自分の設定ファイルがあれば
COPY nginx.conf /etc/nginx/nginx.conf

# コンテンツ
COPY html/ /usr/share/nginx/html/

# オプション：実行時ユーザを明示
USER nginx

# オプション：ヘルスチェック
HEALTHCHECK --interval=10s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost/health || exit 1
```

だいたいこんな感じでカスタマイズして build します。
