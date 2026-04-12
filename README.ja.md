# Docker + Kubernetes Study Repo

Docker と Kubernetes を学ぶための個人用 private repository。日々の学習メモ、短い演習、再実行しやすいコマンドをここに集約する。

## ドキュメント方針

- 日本語の文書は `*.ja.md` を使う
- 日々の進捗は `journal/` に残す
- 実験用の最小コードは `exercises/` に置く
- 現在の到達点は `STATUS.ja.md` に要約する

## 現在の進捗

- 進捗サマリ: `STATUS.ja.md`
- 直近の日報: `journal/2026-04-12.md`

## ゴール

この順で手を動かして理解する。

1. Docker 基礎
2. Docker Compose
3. Kubernetes 基礎
4. ローカルクラスタ運用

## リポジトリ構成

- `journal/`: 日々の進捗ログ
- `notes/`: トピック別メモとチェックリスト
- `exercises/docker/`: Docker 演習
- `exercises/k8s/`: Kubernetes 演習
- `bin/`: 補助スクリプト

## 最初の 1 週間

### Day 1

- Docker Desktop を起動
- `docker run hello-world`
- `notes/00-setup.md` を読む
- `journal/2026-04-12.md` に記録する

### Day 2

- `exercises/docker/01-hello` を実行
- `docker build`, `docker run`, `docker logs` を確認

### Day 3

- `exercises/docker/02-nginx` を実行
- ポート公開と bind mount を確認

### Day 4

- `docker compose` を学ぶ
- 自分の compose 例を追加する

### Day 5

- `kubectl` と `kind` を使う
- `notes/10-k8s-basics.md` を読む

### Day 6

- ローカルクラスタを作る
- `exercises/k8s/01-deployment/deployment.yaml` を apply する

### Day 7

- あいまいだった点を見直す
- `journal/` に週次まとめを書く

## 基本コマンド

### 開始位置

```bash
cd ~/Develop/docker-k8s-study
```

### Docker 確認

```bash
docker --version
docker run hello-world
```

### Kubernetes 確認

```bash
kubectl version --client
kind version
```

## 日次運用

1. 1つ演習するか、1つノートを読む
2. `journal/YYYY-MM-DD.md` に学んだことを書く
3. 小さく commit する

## commit 例

```bash
git add .
git commit -m "study: Docker hello-world and notes"
```

## GitHub へ push

private repository 作成後:

```bash
git remote add origin git@github.com:<your-account>/docker-k8s-study.git
git branch -M main
git push -u origin main
```

# User input
  - Opened Docker Desktop
  - Verified `docker info`
  - Ran `docker run hello-world`
  - Confirmed Docker is working
  - Verified `kubectl` is installed
  - Verified `kind` is installed
