# Docker Exercise 03

## 目的

Dockerfile を自分で書いて、nginx の独自イメージを build し、`docker build` と `docker run` の関係を理解する。

また、演習 02（bind mount）との違いを体験する。

## どういう作りになっているか

- `nginx:alpine` をベースイメージとして使用
- ローカルの `index.html` を build 時にイメージに組み込む
- 独自イメージ `my-custom-nginx` を build
- そのイメージを起動して配信する

## Build

```bash
docker build -t my-custom-nginx:latest .
```

イメージが build されると、`docker images` に表示される。

```bash
docker images | grep my-custom-nginx
```

## Run

```bash
docker run --rm -p 8080:80 my-custom-nginx:latest
```

ブラウザで <http://localhost:8080> を開く。

## 実行前に見るポイント

- 今いる場所が `exercises/docker/03-custom-nginx` か
- `Dockerfile` と `index.html` が同じディレクトリにあるか
- ホスト側の `8080` がすでに他プロセスに使われていないか

確認コマンド:

```bash
pwd
ls -l Dockerfile index.html
```

## この演習の設定の意味

### Dockerfile

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
```

- `FROM nginx:alpine`
  - ベースイメージとして `nginx:alpine` を使う
  - これは公開イメージを pull して、その上に build することを意味する
- `COPY index.html /usr/share/nginx/html/`
  - ホスト側の `index.html` をコンテナ内の nginx 配信ディレクトリにコピー
  - build 時に実行される（実行時ではなく）
  - つまり、ファイルが image に組み込まれる

### docker build コマンド

```bash
docker build -t my-custom-nginx:latest .
```

- `-t my-custom-nginx:latest`
  - build 後の image に付ける名前（tag）
  - `my-custom-nginx` は image 名
  - `latest` はバージョンタグ
- `.`
  - Dockerfile がある場所（現在のディレクトリ）
  - build context という

build が完了すると、`docker images` で確認できる。

### docker run コマンド

```bash
docker run --rm -p 8080:80 my-custom-nginx:latest
```

- `my-custom-nginx:latest`
  - 実行する image 名
  - 演習 02 では `nginx:alpine`（公開イメージ）だったが、ここでは自分で build した image を指定

## サポート観点

この演習で答えられるようにするべき問い:

- 問題は `docker build` か `docker run` か
- Dockerfile のどの行が引っかかっているのか
- image 名が正しいか
- tag と image 名の違いは何か

## 演習 02 との比較

### 演習 02: bind mount（開発向き）

```bash
docker run --rm -p 8080:80 \
  -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" \
  nginx:alpine
```

特徴:

- 既成イメージ `nginx:alpine` をそのまま使う
- `-v` でホスト側のファイルを参照する
- ホスト側のファイルを編集すると、即座に container 側に反映
- image を build する手間がない

### 演習 03: docker build（本番向き）

```bash
# 1. Dockerfile を書く
# FROM nginx:alpine
# COPY index.html /usr/share/nginx/html/

# 2. Image を build
docker build -t my-custom-nginx:latest .

# 3. Image を run
docker run --rm -p 8080:80 my-custom-nginx:latest
```

特徴:

- 独自の image を build する
- ファイルが image に組み込まれる（ビルド時）
- image を他マシンで pull すれば、同じ構成が再現できる
- image 更新には rebuild が必要
- 本番環境・チーム共有に適している

### 使い分けの判断基準

| 用途 | 推奨 | 理由 |
|------|------|------|
| 開発中、ファイル頻繁に変更 | 演習 02 (bind mount) | 変更が即反映、高速サイクル |
| 本番環境、安定動作が必須 | 演習 03 (build) | 構成が固定化、再現性が高い |
| チーム共有、環境統一 | 演習 03 (build) | 同じ image で同じ環境が再現 |
| 複数バージョン同時運用 | 演習 03 (build) | image tag で版管理できる |

## 次に試してみること

### 1. Image のサイズを確認

```bash
docker images my-custom-nginx
```

output:

```
REPOSITORY          TAG       IMAGE ID     CREATED        SIZE
my-custom-nginx     latest    abc123def    2 minutes ago   95MB
nginx:alpine        latest    xyz789uvw    2 weeks ago     91MB
```

independent な `my-custom-nginx` は `nginx:alpine` よりわずかに大きい（`index.html` が追加されるため）。

### 2. Image の详细を確認

```bash
docker image inspect my-custom-nginx:latest
```

JSON 形式で、image の設定、layer 情報、環境変数などが表示される。

### 3. Build ログを見直す

```bash
docker build -t my-custom-nginx:v2 --progress=plain .
```

各ステップの実行内容や、キャッシュの使用状況が表示される。

### 4. Dockerfile を拡張

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
COPY css/ /usr/share/nginx/html/css/
COPY js/ /usr/share/nginx/html/js/
```

複数ファイルやディレクトリを追加して、build し直す。

### 5. カスタム nginx 設定を追加

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

nginx.conf をカスタマイズして COPY する。

## Q&A

### Q. `docker build -t my-custom-nginx:latest .` の `.` はどこのこと？

A. `.` は「現在のディレクトリ（カレントディレクトリ）」を指し、Docker では **build context** として扱われる。

意味としては次の 3 点が重要。

- `.` で指定したディレクトリ配下のファイル群が build 時に Docker へ渡される
- `-f` を省略した場合、既定で `./Dockerfile` が使われる
- `COPY` は context 内のファイルしか参照できない

つまり `exercises/docker/03-custom-nginx` で次を実行すると:

```bash
docker build -t my-custom-nginx:latest .
```

このディレクトリ全体が context になり、`COPY index.html ...` もその範囲からコピーされる。

補足:

- context が大きいと build が遅くなる
- 送らなくてよいファイルは `.dockerignore` で除外する
- context を変えたい場合は `.` の代わりに path を指定できる

例:

```bash
# 1つ上のディレクトリを context にする
docker build -t my-custom-nginx:latest ..

# Dockerfile の場所を明示しつつ、context は現在ディレクトリ
docker build -f Dockerfile -t my-custom-nginx:latest .
```

学習ポイント:

- `.` は「コマンド実行場所」だけでなく「Docker に渡すファイル範囲」の指定
- `COPY` エラーは context の取り違えで起きることが多い

### Q. `docker build -t my-custom-nginx:latest -f ../Dockerfile` で `requires 1 argument` が出るのはなぜ？

A. `-f` は Dockerfile の場所を指定するだけで、**build context 引数**（PATH | URL | -）は別で必須だから。

あなたのコマンドは最後の context が無いため、`docker buildx build requires 1 argument` になる。

誤り（context 不足）:

```bash
docker build -t my-custom-nginx:latest -f ../Dockerfile
```

正しい形（最後に context を付ける）:

```bash
# Dockerfile は ../Dockerfile、context は現在ディレクトリ
docker build -t my-custom-nginx:latest -f ../Dockerfile .

# Dockerfile も context も 1つ上のディレクトリ
docker build -t my-custom-nginx:latest -f ../Dockerfile ..
```

ルールとして覚える形:

```bash
docker build [OPTIONS] <CONTEXT>
```

- `-f` は `<CONTEXT>` の代わりにはならない
- `COPY` が参照できるのは `<CONTEXT>` 内だけ
- まず `pwd` で現在地を確認してから build すると事故が減る

### Q. `docker build` と `docker run` はどう違う？

A. 大きく違う。

- `docker build`
  - Dockerfile から image を作る段階
  - ファイルが image に組み込まれる
  - 実行中ではなく、作成の段階
  - 結果: image ファイル（OCI イメージフォーマット）

- `docker run`
  - image から container を起動する
  - image は変わらず、新しい container インスタンスが作られる
  - 実行の段階
  - 結果: 動いている container プロセス

つまり:

```
Dockerfile
    ↓ (docker build)
image (テンプレート、ファイルシステムスナップショット)
    ↓ (docker run)
container (動いているプロセスと隔離ファイルシステム)
```

### Q. Image と container のサイズはなぜ異なる？

A. image はファイルシステムのスナップショット、container はそれに加えてメモリ上の実行状態を含むから。

- `docker images` で見えるサイズ
  - image の圧縮済みサイズ（layers の合計）
  - 非実行時

- `docker ps` や statistics で見える container size
  - 実行中の write layer（コンテナ固有の変更）+ image の参照
  - 実行時のメモリなどは含まない（`docker stats` で見える）

### Q. 同じ image から複数 container を起動できる？

A. もちろんできる。

```bash
# 同じイメージから複数 container を起動
docker run --rm -p 8080:80 my-custom-nginx:latest
docker run --rm -p 8081:80 my-custom-nginx:latest
docker run --rm -p 8082:80 my-custom-nginx:latest
```

別ターミナルで 3 つ全部走らせると、8080, 8081, 8082 でそれぞれアクセスできる。

### Q. Image を削除するには？

A. `docker rmi` を使う。

```bash
docker rmi my-custom-nginx:latest
```

注意:

- 実行中 container がその image を参照していると削除できない
- その場合は先に `docker stop` や `docker rm` で container を削除

```bash
# 実行中 container から削除
docker ps | grep my-custom-nginx
docker stop <container-id>
docker rmi my-custom-nginx:latest
```

### Q. Build 中に cache が使われているのはなぜ？

A. Docker の build 効率化のため。Dockerfile の各行（RUN, COPY など）がキャッシュされ、次回 build 時に同じ結果が見込まれる行はスキップされる。

例:

```dockerfile
FROM nginx:alpine         # ← layer 1 (cache hit 多い)
RUN apk add --no-cache... # ← layer 2 (通常 cache hit)
COPY index.html ...       # ← layer 3 (index.html が同じなら cache hit)
```

キャッシュを使わずに build:

```bash
docker build --no-cache -t my-custom-nginx:latest .
```

### Q. `tag` って何？

A. Image のバージョンラベル。同じ repository（image 名）でも、異なるバージョンを管理できる。

```bash
docker build -t my-custom-nginx:v1 .
docker build -t my-custom-nginx:v2 .

docker images | grep my-custom-nginx

# Output
# my-custom-nginx   v1    abc123
# my-custom-nginx   v2    def456
```

run する時は tag を指定：

```bash
docker run --rm -p 8080:80 my-custom-nginx:v1
docker run --rm -p 8081:80 my-custom-nginx:v2
```

習慣として:

- `latest` = 最新版（開発用）
- `v1.0`, `v1.1` ... = リリース版（タグで固定）

### Q. Build image を Docker Hub にアップロードできる？

A. できる。`docker push` を使う。

```bash
# 1. Docker Hub にアカウントがあり、ログイン済み
docker login

# 2. Image を Hub 用の名前でビルド
docker build -t <username>/my-custom-nginx:latest .

# 3. Push
docker push <username>/my-custom-nginx:latest

# 4. 他マシンで pull
docker run --rm -p 8080:80 <username>/my-custom-nginx:latest
```

すると、初回は `<username>/my-custom-nginx:latest` が pull され、同じ構成が再現される。

この演習ではここまでやる必要はないが、「image の共有」という Docker の利点は、実は `docker build` と `docker push / pull` で成り立っている。
