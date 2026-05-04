# Q&A

このファイルは、この repository 全体の学習 Q&A を一覧で追うための入口。

## 使い方

- 個別の質問と回答は、関連する演習やノートの Markdown に残す
- このファイルには、その要約と参照先を追記する
- 新しい AI セッションでも、まずここを見れば過去の質問傾向を把握できるようにする

## Docker

### `01-hello`

- Q. `alpine` って何？
  - A. 軽量な Linux ディストリビューションで、Docker のベースイメージとしてよく使われる
  - 詳細: `exercises/docker/01-hello/README.ja.md`

- Q. `alpine` って日本語ではどう発音すればよい？
  - A. `アルパイン` でよい
  - 詳細: `exercises/docker/01-hello/README.ja.md`

### `02-nginx`

- Q. `nginx` って何？
  - A. HTTP リクエストを受けて HTML などを返す Web サーバソフトウェア
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `nginx` ってどう発音するの？
  - A. `エンジンエックス`
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. 一度目の実行と、二度目以降の実行で標準出力が違ったのはなぜ？
  - A. 初回だけ `nginx:alpine` の pull が入りやすく、二度目以降はローカルキャッシュを使うため
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. ローカルって具体的にはどこにダウンロードするの？ それは制御できるの？
  - A. Docker Desktop on macOS では Docker の仮想ディスク `Docker.raw` の中に保存され、個別 image の保存先ではなく disk image 全体の保存場所を設定で動かす
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `Docker.raw` の「中にある」ってどういう意味？
  - A. macOS からは 1 ファイルだが、Docker Desktop の Linux VM からは仮想ディスクとして扱われ、その中の Linux filesystem に Docker データが入る
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. Linux VM は Docker 管理用に必ず作られるの？ スペックは誰が決めるの？
  - A. Docker Desktop on macOS では Linux コンテナを動かすための Docker Desktop 管理 VM が使われ、CPU / memory / disk は Docker Desktop の Resources 設定で管理する
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `-v` は 1 ファイルだけ渡すもの？ HTML + CSS みたいに複数ファイルならどうする？
  - A. `-v` は file でも directory でも mount でき、複数アセットを配信するなら配信ディレクトリごと mount するのが基本
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. HTML にホスト側ポートとコンテナ側ポートを同時表示できる？
  - A. host 側は URL から実測表示できる一方、container 側は通常ブラウザから自動検知できないため設定値として表示するのが実用的
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `-v` でつないだ host 側と container 側の file / directory は確認できる？
  - A. `docker inspect` の `Mounts` で source と destination を見られ、container 内でも `docker exec` で結果を確認できる
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `container-id` ってどうやって知る？
  - A. 基本は `docker ps` で確認し、必要なら `NAMES` も使う。学習中は `PORTS` も一緒に見て対象 container を特定するのが安全
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `IMAGE` と `NAMES` は同じ情報？
  - A. 違う。`IMAGE` はどの image から作ったか、`NAMES` は今動いている container 実体の名前
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `docker ps` 以外で、日常的によく使う Docker コマンドは？
  - A. まずは `docker logs`、`docker inspect`、`docker exec`、`docker images`、`docker run`、`docker stop` あたりが基本
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `docker images` の `In Use` は「今動いている」という意味？
  - A. 必ずしも違う。起動中 container だけでなく、停止済み container が image を参照していても `In Use` になる
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. 停止済みの `hello-world` container を消すには？
  - A. `docker ps -a --filter ancestor=hello-world:latest` で確認してから、`docker rm <container-id>` か `docker rm $(docker ps -aq --filter ancestor=hello-world:latest)` を使う
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `--filter ancestor=` って何？ どう読む？
  - A. Docker フィルターで「このイメージから作られた container」を絞り込む。`ancestor` は「祖先」という意味の英語で、イメージが container の親世代にあたるから
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. nginx コンテナは `/usr/share/nginx/html/index.html` をトップ HTML として Web サーバを立ち上げてくれるもの。そのようにコンテナが出来上がっているってことでいいのか？
  - A. その理解で正しい。`nginx:alpine` は既成の image で、Dockerfile で nginx インストール → ポート 80 → `/usr/share/nginx/html/` 配信対象のように既に設定された出来上がった state で public されている
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. 別のディレクトリの `index.html` をトップにしたい nginx container を作りたい場合はどうする？
  - A. 2 つの方法がある：1) bind mount で差し替える（開発向き、動的反映）2) Dockerfile を書いて build する（本番向き、image に固定化）
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `docker build -t my-custom-nginx:latest .` の `.` はどこのこと？
  - A. 現在ディレクトリを指す build context。Docker に送るファイル範囲を表し、`COPY` はその範囲内しか参照できない
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

- Q. `docker build -t my-custom-nginx:latest -f ../Dockerfile` で `requires 1 argument` になるのはなぜ？
  - A. `-f` は Dockerfile の場所指定のみで、最後の build context 引数（例: `.`）が必須。`docker build [OPTIONS] <CONTEXT>` の形にする必要がある
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

- Q. Pod 名の末尾のハッシュ部分（例: `hello-nginx-d54d8664b-6dhqx` の `-d54d8664b-6dhqx`）はどこから？
  - A. `d54d8664b` は ReplicaSet が自動生成する ID、`6dhqx` は Pod ごとのランダムサフィックス。`kubectl get pods` の出力で full name が確認できる
  - 詳細: `exercises/k8s/01-deployment/README.ja.md`

- Q. `kubectl logs`, `kubectl describe pod`, `journalctl` の違いは？
  - A. ログ 3 層：(1) `kubectl logs` = コンテナの stdout/stderr、(2) `kubectl describe pod` = Pod のライフサイクルイベント、(3) `journalctl` = ノードの OS レベル kubelet/containerd ログ。最初の 2 つでサポートの 99% が解決する
  - 詳細: `exercises/k8s/01-deployment/README.ja.md`

- Q. ログは実際のところどこに保存されているのか？ `kubectl logs` はノード上のファイルから読むのか？
  - A. Kubernetes はコンテナの stdout/stderr をストリームとしてキャプチャし、ファイルではない形で保持。ノード上の `/var/log/pods/` に一時保存されるが、これは実装詳細。`kubectl logs` は API を経由して抽象化している
  - 詳細: `exercises/k8s/01-deployment/README.ja.md`

- Q. `COPY index.html ...` で `"/index.html": not found` になるのはなぜ？
  - A. build context に `index.html` が含まれていないため。`-f` で Dockerfile を読めても、`COPY` は context 内しか参照できない
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

- Q. `transferring context: 2B` の `2B` は容量の意味？
  - A. はい。2 bytes を意味し、Docker に送られた build context がほぼ空であるサイン
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

- Q. build したいファイルが複数ディレクトリに散在している時はどうする？
  - A. 一時ディレクトリに集約して build は有効。実務では repo ルート context + `.dockerignore`、または staging copy、上級では `--build-context` を使う。symlink より copy 集約が再現性高い
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

- Q. context 内のファイルをコンテナ内の別々のパスに置いて build できる？
  - A. できる。`COPY <src> <dest>` を複数書けば、1つの context から複数の配置先へ展開できる（これが標準）
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

- Q. Build 時にファイルの権限（permission）も制御できる？オリジナルの情報をコピーするだけ？
  - A. 制御できる。`COPY --chown` で所有権、`RUN chmod`/`RUN chown` で権限を設定可能。ホスト側の権限は自動では保持されない
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

- Q. `ERROR: failed to connect to the docker API at unix:///Users/.../docker.sock` と言われる場合は？
  - A. Docker デーモン（サーバー）が起動していない。Docker Desktop 等を起動して「クライアント」からの接続を受け付けられる状態にする
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

- Q. ターミナル上で Docker デーモンが起動しているか状態を確認するには？
  - A. `docker version` で `Client` と `Server` の両方が出力されるか見るか、`docker info` で詳細情報が出力されるかで確認するのが確実
  - 詳細: `exercises/docker/03-custom-nginx/README.ja.md`

### `04-compose`

- Q. `docker compose up` は compose ファイルがあるディレクトリ以外からでも指定して実行できる？ その場合 volumes の相対パスはどうなる？
  - A. `-f` オプションで別ディレクトリから指定可能。volumes の相対パスは「コマンド実行場所」ではなく「compose ファイルのあるディレクトリ」を基準に解釈されるため、どこから実行しても安全に再現できる
  - 詳細: `exercises/docker/04-compose/README.ja.md`

## Docker References

- nginx:alpine の Dockerfile 構成と仕組み
  - 詳細: `notes/nginx-dockerfile.ja.md`
  - 公式イメージの構造、各行の意味、デーモン管理の理由、カスタム Dockerfile テンプレート

- Q. コンテナのデフォルトファイル状態を確認するには？
  - A. `docker exec <container-id> ls /path` でコンテナ内の directory を見る。`docker inspect` で mount 情報、`docker stat` で file metadata を確認
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

- Q. `docker exec` するには、コンテナが running していないといけない？bind mount されているファイルは `docker exec` で見るとどう表示される？
  - A. その通り。`docker exec` は running container に対してのみ実行可能。bind mount されたファイルは、ホスト側の現在の内容がそのまま見える（mount されているから変更が反映）
  - 詳細: `exercises/docker/02-nginx/README.ja.md`

## Collaboration

- Q. この AI 協働の質は世間一般と比べてどの程度？
  - A. 一般的な単発利用よりかなり高く、継続的な作業パートナー運用としてはかなり成熟している
  - 詳細: `COLLABORATION_GUIDE.ja.md`

### `advanced` (その他応用)

- Q. Docker コンテナ（イメージ）の中で、さらに別の Docker コンテナを動かすことはできる？
  - A. 可能です。主に2つの方法があります。1つは親の `docker.sock` をマウントして親に作らせる「DooD (Docker out of Docker)」。もう1つはコンテナ内で完全に独立したデーモンを動かす特権的な「DinD (Docker in Docker)」です。この技術は、これから学ぶ `kind` (Kubernetes in Docker) のコア設計そのものです。
  - 詳細: 次の Kubernetes 基礎 (`kind`) の学習で体感します。

## Kubernetes

- Q. Kubernetes (`k8s`) と `k3s` の違いは何？ サポートエンジニアとして知っておくべき？
  - A. `k8s` は標準（源流）。`k3s` はクラウド固有のコード等を削ぎ落とし、1つのバイナリにまとめた軽量で完全互換な K8s ディストリビューション。「K8s は規格（API）であり、k3s, EKS, kind など多様な実装がある」と知っておくのは顧客サポート・切り分けの前提として非常に重要
  - 詳細: 今後 `notes/` や Day 5 の学習にて解説予定

- Q. `k3s` とは何で、`kind` とはどう違う？
  - A. `k3s` はエッジや低リソース環境向けに設計された軽量な Kubernetes ディストリビューションです。`kind` は upstream Kubernetes を Docker 上で動かすローカルクラスタツールです。どちらも標準の Kubernetes API をサポートしますが、k3s はディストリビューション、kind はクラスタ作成ツールという位置づけです。
  - 詳細: `notes/k3s-basics.ja.md`

- Q. `kind` が kubeconfig に cluster, user, context を追加するってどういう意味？
  - A. kubeconfig (~/.kube/config) は Kubernetes の接続設定を保存。`cluster` は API エンドポイント、`user` は認証、`context` はそれらを組み合わせた接続設定。
  - 詳細: `notes/10-k8s-basics.ja.md`

## 更新ルール

- 質問が出たら、関連する Markdown に詳細な Q&A を追記する
- そのあと、この `QA.ja.md` にも要約を追加する
