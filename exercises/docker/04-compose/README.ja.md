# Docker Exercise 04: Docker Compose

## 目的

`docker run` で毎回長いオプション（`-p 8080:80 -v ...`）を打つのではなく、`compose.yaml` という設定ファイルに構成を記述して、インフラのコード化（IaC: Infrastructure as Code）の基礎を学ぶ。

## compose.yaml とは

Docker Compose は、複数コンテナのアプリケーションを定義して実行するためのツール（現在は Docker CLI に統合され `docker compose` として使える）。
1つのコンテナを動かすだけでも、起動パラメータをファイルとして保存・共有できるため非常に便利。

## はじめての Compose

この演習では、演習 02 で学んだ bind mount を伴う nginx の起動を `compose.yaml` で再現する。

### 1. 準備

このディレクトリで以下のコマンドを実行し、`index.html` と `compose.yaml` があることを確認する。

```bash
ls -l
```

### 2. 起動 (Up)

以下のコマンドでコンテナを起動する（`-d` はバックグラウンド実行を意味する detached モード）。

```bash
docker compose up -d
```

ブラウザで <http://localhost:8080> を開く。

### 3. 確認

動いているコンテナの一覧を確認する。

```bash
docker compose ps
```

### 4. 終了 (Down)

起動したコンテナ群をまとめて停止・削除する。

```bash
docker compose down
```

## サポート観点

この演習で答えられるようにするべき問い:

- `docker run` と `docker compose` の違いは何か
- `compose.yaml` を使うことでどんなメリットがあるか
- `compose.yaml` 内の `ports` や `volumes` は `docker run` のどのオプションに対応するか

## 次にやること

1. 手元のターミナルで対象ディレクトリに移動する: `cd ../04-compose`
2. 上の「はじめての Compose」のステップ 1〜4 を手元で一通り実行してみる
3. 実行後、コマンドを打った際の `docker run` との違いや、何が楽になったと感じたか、感想をまとめる

## Q&A

### Q. `docker compose up` 実行時、`compose.yaml` は必ずカレントディレクトリにある必要がある？
A. いいえ。`-f`（または `--file`）オプションを使って別の場所にある compose ファイルを指定できます。

```bash
docker compose -f /path/to/my-project/compose.yaml up -d
```

### Q. 別のディレクトリから `-f` で実行した場合、`volumes` に書かれた相対パス（例: `./index.html`）はどう解釈される？
A. **`compose.yaml` が置かれているディレクトリを基準** に解釈されます（※コマンドを実行したカレントディレクトリ基準ではありません）。

これはインフラをコード化（IaC）する上で非常に優れた設計です。ファイルの中に `./index.html` と書いておけば、ユーザーがどこから `docker compose -f` コマンドで呼び出そうとも、必ず「compose ファイルの隣にある index.html」がマウントされます。つまり、**実行場所に依存せず環境が再現される**セキュアな仕組みになっています。
