# Docker + Kubernetes Support Study Repo

Docker と Kubernetes を、将来サポートする立場のエンジニアとして学ぶための個人用 private repository。単にコマンドをなぞるのではなく、構造、設定、責務分担、トラブル時の見方まで理解することを目的にする。

## 最初に見る場所

- 現在地の要約: `STATUS.ja.md`
- 英語の入口: `README.md`
- 引き継ぎ要約: `HANDOFF.ja.md`
- 運用ルール: `REPOSITORY_POLICY.ja.md`
- AI 向け指示書: `AGENTS.md`
- 直近の日報: `journal/2026-04-12.md`

## この repository の学習姿勢

各演習では、必ず次の観点を残す。

1. これは何の役割を持つか
2. どこで動いているか
3. どの設定ファイルがそれを制御しているか
4. どう確認するか
5. どこが壊れやすいか

## ドキュメント方針

- 日本語の文書は `*.ja.md` を使う
- 日々の進捗は `journal/` に残す
- 実験用の最小コードは `exercises/` に置く
- 現在の到達点は `STATUS.ja.md` に要約する
- commit message は Conventional Commits に従う

## 現在の進捗

- 進捗サマリ: `STATUS.ja.md`
- 直近の日報: `journal/2026-04-12.md`
- 現在の次アクション: `STATUS.ja.md` の「次にやること」

## ゴール

この順で、運用やサポートに必要な理解まで含めて身につける。

1. Docker 基礎
2. Docker Compose
3. Kubernetes 基礎
4. ローカルクラスタ運用
5. トラブル時の切り分け

## リポジトリ構成

- `journal/`: 日々の進捗ログ
- `notes/`: トピック別メモとチェックリスト。日本語と英語の両方を置く
- `exercises/docker/`: Docker 演習。設定ファイルの意味も説明する
- `exercises/k8s/`: Kubernetes 演習。Manifest の各項目の意味も説明する
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
2. `journal/YYYY-MM-DD.md` に、作業結果だけでなく「構造」と「設定の意味」を書く
3. 小さく commit する

## 文書ルール

- 日本語文書は `*.ja.md`
- 主要な概念説明は日本語と英語の両方で残す
- 演習 README には必ず「この設定が何を意味するか」を書く
- 進捗だけでなく、サポート観点のメモを残す

## commit ルール

```bash
git add .
git commit -m "docs: update study progress"
```

Conventional Commits の型:

- `docs:` ドキュメント更新
- `feat:` 新しい学習用機能や補助スクリプト追加
- `fix:` 誤り修正
- `chore:` 雑務や初期整備
- `refactor:` 構造整理

## GitHub へ push

private repository 作成後:

```bash
git remote add origin git@github.com:<your-account>/docker-k8s-study.git
git branch -M main
git push -u origin main
```
