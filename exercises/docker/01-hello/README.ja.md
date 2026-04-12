# Docker Exercise 01

## 目的

最小のカスタムイメージを作り、Dockerfile が何を定義しているのかを理解する。

## どういう作りになっているか

- `Dockerfile` はイメージの作り方を定義する
- `FROM alpine:3.22` はベースイメージを決める
- `CMD [...]` は、そのイメージから起動したコンテナのデフォルト実行コマンドを決める

この演習は、各命令の役割を見やすくするために意図的に最小構成にしている。

## Build

```bash
docker build -t study-hello .
```

## Run

```bash
docker run --rm study-hello
```

## この設定の意味

- `FROM alpine:3.22`
  - Alpine Linux `3.22` を土台にする
- `CMD ["echo", "hello from docker-k8s-study"]`
  - 明示的に上書きしない限り、このコマンドが起動時に実行される
- `-t study-hello`
  - イメージに分かりやすいタグを付ける
- `--rm`
  - コンテナ終了後に自動削除する

## サポート観点

この演習で答えられるようにするべき問い:

- 問題はビルド段階か、実行段階か
- コンテナは実際に何を実行しているか
- `docker run` で `CMD` を上書きすると何が変わるか
