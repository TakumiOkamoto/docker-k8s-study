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

## Collaboration

- Q. この AI 協働の質は世間一般と比べてどの程度？
  - A. 一般的な単発利用よりかなり高く、継続的な作業パートナー運用としてはかなり成熟している
  - 詳細: `COLLABORATION_GUIDE.ja.md`

## Kubernetes

- まだ Q&A なし

## 更新ルール

- 質問が出たら、関連する Markdown に詳細な Q&A を追記する
- そのあと、この `QA.ja.md` にも要約を追加する
