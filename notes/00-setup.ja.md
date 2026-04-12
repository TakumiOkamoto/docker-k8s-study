# セットアップ

## この章の目的

この章は「Docker と Kubernetes を使えるようにする」だけではなく、「どの部品が何を担当しているか」を理解するための入口。

## 構成要素

### Docker Desktop

macOS 上で Linux コンテナを動かすための実行基盤。Docker CLI そのものではなく、実際にコンテナを動かす Docker Engine や関連コンポーネントをまとめて提供する。

### Docker CLI

`docker` コマンド。利用者はこれを叩くが、実際の処理は Docker Desktop 側の daemon に依頼する。

### kubectl

Kubernetes API に対するクライアント。クラスタの状態取得や manifest の apply に使う。

### kind

Kubernetes in Docker。Docker コンテナの中にローカル Kubernetes クラスタを作る。学習や検証に向く。

## インストール

```bash
brew install --cask docker
brew install kubectl kind
```

任意:

```bash
brew install k9s helm
```

## 何を確認しているか

```bash
docker --version
kubectl version --client
kind version
```

- `docker --version`
  - CLI が入っていることの確認
- `kubectl version --client`
  - Kubernetes クライアントが入っていることの確認
- `kind version`
  - ローカルクラスタ作成ツールが使えることの確認

## Docker Desktop を一度起動する意味

macOS では `docker` コマンドだけ入っていても、daemon が動いていなければコンテナは起動できない。Docker Desktop を起動するのは、背後の Docker Engine を利用可能にするため。

## 最初の疎通確認

```bash
docker run hello-world
```

このコマンドで確認していること:

- CLI から daemon に接続できる
- イメージ取得ができる
- コンテナが起動できる
- 標準出力を受け取れる

## 早い段階で混同しやすい点

- Docker image と container は別物
- Docker CLI と Docker Engine は別物
- `kubectl` が入っていてもクラスタがあるとは限らない
- `kind` が入っていてもクラスタが起動しているとは限らない
