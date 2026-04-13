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

## 実行前に見るポイント

- 今いる場所が `exercises/docker/02-nginx` か
- mount 元の `index.html` がそのディレクトリにあるか
- ホスト側の `8080` がすでに他プロセスに使われていないか

確認コマンド:

```bash
pwd
ls -l index.html
```

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

## どこで何が起きているか

- ホスト
  - `index.html` を持っている
  - ブラウザから `localhost:8080` にアクセスする
- コンテナ
  - nginx が `80` 番ポートで待ち受ける
  - `/usr/share/nginx/html/index.html` を配信する
- Docker のポート公開
  - ホスト `8080` へのアクセスをコンテナ `80` に転送する

つまり、この演習は 1 つのコマンドで 3 層を同時に触っている。

## 実行後の確認

1. ターミナルに nginx のエラーが出ていないかを見る
2. ブラウザで <http://localhost:8080> を開く
3. 表示内容が `index.html` の中身と一致するか確認する

別ターミナルで確認したい場合:

```bash
docker ps
docker logs <container-id>
```

## 切り分けの順番

### 1. ページが開かない

- `docker run` 自体が終了していないか
- `-p 8080:80` を付け忘れていないか
- ホスト側の `8080` が競合していないか

### 2. nginx のページは出るが、期待した HTML ではない

- `"$PWD/index.html"` が正しい場所を指しているか
- mount 先 `/usr/share/nginx/html/index.html` が正しいか
- カレントディレクトリを取り違えていないか

### 3. コンテナは動くが内容更新が反映されない

- mount が file 単位で入っているか
- ブラウザキャッシュを見ていないか
- 実際に編集したのが mount 元ファイルか

## この演習の完了条件

- `index.html` の内容がブラウザに表示される
- `-p 8080:80` の意味を言葉で説明できる
- bind mount が「コピー」ではなく「ホストのファイルを見せる仕組み」だと言える
- 問題が起きた時に、ホスト、コンテナ、ポート公開のどこを見るか順番を言える

## Q&A

### Q. `nginx` って何？

A. `nginx` は Web サーバソフトウェア。ブラウザから来た HTTP リクエストを受け取り、HTML ファイルなどを返す役割を持つ。

この演習では `nginx:alpine` を使っているので、Docker Hub に公開されている nginx イメージをそのまま使っている。

この文脈で押さえるべき点:

- `nginx` はコンテナの中で動く Web サーバ
- `80` 番ポートで待ち受ける
- `/usr/share/nginx/html/` 配下のファイルを配信する設定がデフォルトで入っている
- 今回はその `index.html` を bind mount で差し替えている

つまり、この演習は「nginx を作る」演習ではなく、「既成の Web サーバを使ってポート公開と mount を理解する」演習。

### Q. `nginx` ってどう発音するの？

A. 日本語では普通に `エンジンエックス` と読めばよい。

補足:

- 英字は `N-G-I-N-X` と書く
- 会話では `nginx` より `エンジンエックス` と読んだほうが伝わりやすい
- 少なくとも日本語の技術会話では `ンギンクス` のようには読まない

### Q. 一度目の実行と、二度目以降の実行で標準出力が違ったのはなぜ？

A. いちばん大きい理由は、初回だけ `nginx:alpine` イメージの取得が必要だから。

`docker run nginx:alpine` は内部的には大きく 2 段階ある。

1. ローカルにイメージがあるか確認する
2. そのイメージからコンテナを起動する

初回実行時はローカルに `nginx:alpine` がまだ無いので、Docker はレジストリからイメージを pull する。そのため標準出力や進捗表示に、次のような「取得中」の情報が出やすい。

- `Unable to find image ... locally`
- `Pulling from library/nginx`
- layer の download / extract 進捗

二度目以降は、そのイメージがローカルにキャッシュされていれば pull は不要なので、その部分の出力が消える。見えるのは主に「コンテナ起動後」の出力になる。

この演習での理解ポイント:

- 初回は「イメージ取得 + コンテナ起動」
- 二度目以降は「コンテナ起動」だけになりやすい
- 出力の違いは、コンテナの中身が変わったというより、`docker run` の前半処理が省略されたことによる

確認したい時は次も見るとよい。

```bash
docker images
docker image inspect nginx:alpine
```

### Q. ローカルって具体的にはどこにダウンロードするの？ それは制御できるの？

A. Docker Desktop on macOS では、イメージは host の通常ディレクトリにそのまま展開されるわけではない。Docker Desktop が管理する Linux VM 用の大きな disk image ファイルの中に保存される。

この Mac では、実ファイルとして次が見えている。

```text
/Users/tapacchi/Library/Containers/com.docker.docker/Data/vms/0/data/Docker.raw
```

つまり `nginx:alpine` も、この `Docker.raw` の中に入る。

制御については整理するとこうなる。

- Docker Desktop on macOS
  - 個々の image を「このフォルダへ置く」とは通常指定しない
  - 代わりに Docker Desktop の `Disk image location` 設定で、disk image 全体の保存場所は変更できる
- Docker Engine on Linux
  - daemon の `data-root` で保存先ディレクトリを変更できる
  - 典型的な既定値は `/var/lib/docker`

この文脈で重要なのは、Docker Desktop on macOS では「image 単位の保存先」より「Docker Desktop 全体の仮想ディスクの置き場所」を管理する、ということ。

### Q. `Docker.raw` の「中にある」ってどういう意味？

A. `Docker.raw` は、macOS から見ると 1 つの大きなファイルだが、Docker Desktop の Linux VM から見ると 1 台の仮想ディスクとして扱われる。

その仮想ディスクの中に Linux のファイルシステムがあり、その中へ Docker の image layer、container の書き込み layer、volume などが保存される。

イメージ:

```text
macOS
└─ Docker.raw
   └─ Docker Desktop が使う仮想ディスク
      └─ Linux filesystem
         ├─ image layers
         ├─ container writable layers
         └─ volumes
```

つまり、`nginx:alpine` が host の普通のフォルダとして見えているわけではなく、Docker Desktop が管理する Linux 側ストレージの中にある、という意味。

### Q. Linux VM は Docker 管理用に必ず作られるの？ スペックは誰が決めるの？

A. Docker Desktop on macOS では、Linux コンテナを動かすために Docker Desktop 管理の Linux VM が使われる。

理由は、Linux コンテナは Linux カーネル前提で動くが、host は macOS だから。そのため Docker Desktop が Linux 環境を VM として用意し、その中で Docker Engine と container を動かす。

流れを簡略化するとこうなる。

```text
macOS terminal
-> docker CLI
-> Docker Desktop
-> Docker Desktop managed Linux VM
-> Docker Engine
-> containers
```

スペックについては:

- 初期値は Docker Desktop が決める
- 必要なら利用者が Docker Desktop の Resources 設定で CPU / memory / swap / disk を調整する

この Mac でも `docker context ls` に `desktop-linux` が見えているので、Docker Desktop 管理の Linux 環境を使っていると分かる。

### Q. `-v` は 1 ファイルだけ渡すためのもの？ HTML + CSS のように複数ファイルがある時は？

A. `-v` は「ファイル」でも「ディレクトリ」でも mount できる。いまの演習コマンドは file mount なので `index.html` だけを差し込んでいる。

そのため、`index.html` が `style.css` や `app.js` を参照している場合は、次のどちらかが必要。

- CSS/JS も個別に file mount する
- 配信ディレクトリごと mount する

静的サイトでは、通常はディレクトリごと mount する方が自然。

例:

```bash
docker run --rm -p 8080:80 -v "$PWD:/usr/share/nginx/html:ro" nginx:alpine
```

この形なら、`index.html` からの相対参照 (`./style.css` など) もそのまま解決される。

整理すると:

- file mount は「1 ファイルだけ差し替えたい」とき向き
- directory mount は「HTML/CSS/JS まとめて配信したい」とき向き

### Q. HTML 上に「ホスト側ポート」と「コンテナ側ポート」を同時表示できる？

A. 学習用としては「表示できる」。ただし、表示の性質は 2 つで違う。

- ホスト側ポート
  - ブラウザの URL から取得できる (`window.location.port`)
- コンテナ側ポート
  - ブラウザから自動検知は基本できない
  - nginx 設定や `docker run -p` の意図として表示する（例: `80`）

つまり「完全に両方を実測」ではなく、

- 片方は実測（host 側）
- 片方は設定値の可視化（container 側）

という形になる。

この演習の `index.html` では実際にこの形式で表示しており、`host 8080 -> container 80` の対応を画面で確認できるようにしてある。

### Q. 同じ考え方で、`-v` でつないだ host 側と container 側の file / directory を確認できる？

A. 確認できる。ただし、これはブラウザではなく Docker CLI 側で見るのが基本。

いちばん分かりやすいのは `docker inspect` の `Mounts`。

流れ:

```bash
docker ps
docker inspect <container-id>
```

ここで `Mounts` を見ると、少なくとも次が確認できる。

- host 側の path (`Source`)
- container 側の path (`Destination`)
- file mount か bind mount かなどの種別 (`Type`)
- 読み取り専用かどうか (`RW`)

見やすく絞るなら次でもよい。

```bash
docker inspect <container-id> --format '{{range .Mounts}}{{println .Type .Source "->" .Destination "RW=" .RW}}{{end}}'
```

さらに container 側からも確認できる。

```bash
docker exec <container-id> ls -l /usr/share/nginx/html
```

file mount なら、その file が container 内のどこに見えているかを確認できる。directory mount なら、その directory 配下の複数 file が見える。

ただし、HTML / browser から host 側 path を知るのは基本できない。これは port の話と似ていて、browser が見える範囲と Docker が管理している範囲が分かれているから。

整理すると:

- browser
  - container から返されたコンテンツは見える
  - host 側の mount 元 path は見えない
- Docker CLI
  - host 側 path と container 側 path の対応を見られる
- container 内
  - mount された結果として見えている file / directory を確認できる

### Q. `container-id` ってどうやって知る？

A. いちばん基本は `docker ps`。

```bash
docker ps
```

この一覧に、起動中 container の次が出る。

- `CONTAINER ID`
- `NAMES`
- `PORTS`

たとえば nginx 演習を起動中なら、`PORTS` に `0.0.0.0:8080->80/tcp` のような表示が出るので、その行の `CONTAINER ID` を使えばよい。

例:

```bash
docker inspect 1a2b3c4d5e6f
docker exec 1a2b3c4d5e6f ls -l /usr/share/nginx/html
```

`container-id` の代わりに `NAMES` も使える。

```bash
docker exec <container-name> ls -l /usr/share/nginx/html
```

より扱いやすくするなら、起動時に名前を付ける方法もある。

```bash
docker run --rm --name study-nginx -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine
```

この場合は `container-id` を覚えなくても、`study-nginx` で `docker inspect` や `docker exec` ができる。

短く ID だけ欲しいなら次も便利。

```bash
docker ps -q
```

ただし container が複数動いていると、どれの ID か分かりにくくなる。学習中は `docker ps` で `PORTS` や `NAMES` も一緒に見る方が安全。

### Q. `IMAGE` と `NAMES` は同じ情報に見えるけど、違いは？

A. 違う。たまたま似た文字列になることはあるが、意味は別。

- `IMAGE`
  - どの image から container を作ったか
  - 例: `nginx:alpine`
- `NAMES`
  - その起動中 container 自体についた名前
  - 例: `study-nginx` や Docker が自動生成した名前

つまり:

- `IMAGE` は「設計図・元データ」寄り
- `NAMES` は「今動いている実体」寄り

同じ image から複数 container を起動できるので、`IMAGE` は同じでも `NAMES` は別になる。

例:

```bash
docker run --rm --name web-a -p 8080:80 nginx:alpine
docker run --rm --name web-b -p 8081:80 nginx:alpine
```

この場合:

- `IMAGE` は両方とも `nginx:alpine`
- `NAMES` は `web-a` と `web-b`

`docker exec` や `docker inspect` の対象を指定するときは、ふつう `NAMES` か `CONTAINER ID` を使う。`IMAGE` は「どの種類の container か」を知るための情報。

### Q. `docker ps` みたいに、Docker で日常的によく使う標準コマンドって他にある？

A. ある。まずは次を押さえると十分実用になる。

状態確認:

- `docker ps`
  - 起動中 container を見る
- `docker ps -a`
  - 停止済みも含めて container 一覧を見る
- `docker images`
  - ローカルにある image 一覧を見る

中身・状態の確認:

- `docker logs <name-or-id>`
  - container の標準出力 / 標準エラーを見る
- `docker inspect <name-or-id>`
  - 詳細設定、port、mount、network などを JSON で見る
- `docker exec -it <name-or-id> sh`
  - container の中に入って確認する

起動・停止・削除:

- `docker run ...`
  - 新しい container を起動する
- `docker stop <name-or-id>`
  - 起動中 container を止める
- `docker rm <name-or-id>`
  - 停止済み container を削除する

image 操作:

- `docker build -t <image-name> .`
  - Dockerfile から image を作る
- `docker pull <image>`
  - image を取得する

この演習の流れに寄せると、最初に特に使うのは次の 6 個。

```bash
docker ps
docker logs <name-or-id>
docker inspect <name-or-id>
docker exec -it <name-or-id> sh
docker images
docker run ...
```

考え方としては:

- `run`
  - 作って動かす
- `ps`
  - 今どうなっているか見る
- `logs`
  - エラーや起動メッセージを見る
- `inspect`
  - port / mount / network の事実を確認する
- `exec`
  - container の中から見る
- `images`
  - 元になる image を見る

この 6 個をまず自然に使えるようになると、Docker の基本操作はかなり前に進む。

### Q. `docker images` の `In Use` や `U` は、「その image が今動いている」という意味？

A. 必ずしもそうではない。`In Use` は、その image を参照している container が存在する、という意味で読むのが安全。

つまり:

- 起動中 container が使っている場合もある
- 停止済み container が残っていて使っている場合もある

この Mac では実際に、`hello-world` は `In Use` だが起動中ではなかった。確認結果はこうだった。

```bash
docker ps -a --filter ancestor=hello-world:latest
```

結果:

- `hello-world` の container が 3 つ残っている
- すべて `Exited (0)`
- つまり「実行中ではないが、停止済み container が image を参照している」状態

一方で `nginx:alpine` は起動中 container が 1 つあり、こちらは本当に動いていた。

確認の考え方:

- `docker images`
  - image が使われているかの大まかな把握
- `docker ps`
  - 今まさに起動中か確認
- `docker ps -a`
  - 停止済みも含め、どの container が参照しているか確認

もし停止済みの `hello-world` container を消したければ、たとえば次を使う。

```bash
docker ps -a --filter ancestor=hello-world:latest
docker rm <container-id>
```

複数あるなら、対象を確認したうえでまとめて削除してもよい。

### Q. 停止済みの `hello-world` container を実際に消すには？

A. いちばん安全なのは、まず一覧を見てから `docker rm` で消すやり方。

確認:

```bash
docker ps -a --filter ancestor=hello-world:latest
```

この Mac では今、次の 3 つが残っている。

- `70b6ede64a52`
- `2ab3e021c407`
- `537bd7d759a7`

1 個ずつ消すなら:

```bash
docker rm 70b6ede64a52
docker rm 2ab3e021c407
docker rm 537bd7d759a7
```

まとめて消すなら:

```bash
docker rm $(docker ps -aq --filter ancestor=hello-world:latest)
```

削除後に確認:

```bash
docker ps -a --filter ancestor=hello-world:latest
docker images
```

学習上のポイント:

- `docker rm`
  - container を消す
- `docker rmi`
  - image を消す

まずは container を消して、`hello-world` の `In Use` 表示がどう変わるかを見るのがよい。

注意:

- `docker rm` は停止済み container に使う
- 起動中なら先に `docker stop <name-or-id>` が必要

### Q. `--filter ancestor=` って何？ どう読む？

A. Docker フィルターオプションの 1 つで、「このイメージから作られた container」を絞り込む。

読み方:

- `ancestor` = 「祖先」「親」という意味の英語
- 日本語では「アンセスター」と読む

使い方を整理すると:

- イメージ = container のテンプレート（親）
- container = イメージから作られた実体（子）
- つまり、イメージは container の「祖先」

例:

```bash
# hello-world イメージから作られた container をすべて表示
docker ps -a --filter ancestor=hello-world:latest

# nginx:alpine イメージから作られた起動中 container をすべて表示
docker ps -a --filter ancestor=nginx:alpine
```

この演習では、`--filter ancestor=hello-world:latest` で「hello-world から作られた container」を絞り込んで、どの container を削除するか指定するときに使っている。

他の filter との組み合わせ:

- `--filter ancestor=<image>` 単独
  - そのイメージから作られたすべての container（起動中・停止済み）
- `--filter "ancestor=<image>" --filter "status=exited"`
  - そのイメージから作られた停止済み container だけ

学習上のポイント:

- image と container の親子関係を理解する取っ掛かり
- filter 的には「image by name」で container を逆引きする手段

### Q. nginx コンテナは、`/usr/share/nginx/html/index.html` をトップ HTML として、Web サーバを立ち上げてくれるもの。そのようにコンテナが出来上がっているってことでいいのか？

A. その理解で正しい。`nginx:alpine` は既成の image であり、Dockerfile ですでに「nginx をインストール」→「ポート 80 で待ち受け」→「'/usr/share/nginx/html/' ディレクトリを配信対象」というように設定された出来上がった state で public されている。

つまり:

- 我々は `docker run` で起動するだけ
- コンテナは起動時に自動で nginx を実行
- nginx は `/usr/share/nginx/html/` の中を配信対象にして、ポート 80 で request を待つ

この演習での役割分担:

```
nginx:alpine イメージ（既成品）
└─ nginx が起動して `/usr/share/nginx/html/` を配信

我々がやること
└─ `-p 8080:80` でホスト 8080 → container 80 に接続
└─ `-v "$PWD/index.html:/usr/share/nginx/html/index.html:ro"` で
   ホスト側ファイルを、nginx が配信するファイルに差し替える
```

Dockerfile 側の簡略版イメージ:

```dockerfile
FROM alpine:latest
RUN apk add nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

- nginx をインストール
- ポート 80 を公開
- container 起動時に `nginx ...` を自動実行

つまり、この image を pull して `docker run` すれば、その時点で既に Web サーバとして動く、という設計になっている。

学習結論:

- 既成 image は「そのように出来上がった状態」で public されている
- ユーザーは「その image の使い方」に合わせて `-p` や `-v` を指定する
- だから「nginx:alpine とは何か」と「ポート 80 とは何か」を最初から理解すると、使い方の意図が見えやすくなる
