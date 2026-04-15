# Handoff

## この repository について

この repository は、利用者が Docker と Kubernetes を学びつつ、将来それらをサポートするエンジニアとして説明・切り分けできる理解を作るための private repository。

## 利用者の意図

- 単に手順をなぞるのではなく、構造と設定の意味を理解したい
- Docker / Kubernetes を「使う側」ではなく「支える側」に近い視点で学びたい
- 日々の進捗を Markdown で追跡したい
- 明日以降は別の AI セッションでも、自然に続きを再開したい

## AI に期待される役割

- 信頼できる学習パートナーとして振る舞う
- 次に何をやるかを具体的に示す
- 進捗、要点、Q&A を Markdown に残す
- 設定ファイルや manifest の意味を必ず説明する
- `git push` 直前までの編集と commit を担当する

## 重要な運用ルール

- 日本語文書は `*.ja.md`
- すべての Markdown 文書は `.md` と `.ja.md` の両方を持つ
- 主要な説明は日本語と英語の両方で整備する
- 利用者から学習上の質問が出たら、関連する Markdown に Q&A として追記する
- commit message は Conventional Commits に従い、英語で書く
- 現在地は `STATUS.ja.md` を見れば分かるように保つ

## この turn までの進捗

- 学習用 repository を作成済み
- GitHub remote を設定済み
- Docker Desktop は起動確認済み
- `docker info` 実行によるクライアント・サーバーの疎通確認
- `kubectl` / `kind` 利用可能
- Dockerfile 最小演習 `01-hello` 完了
- Docker bind mount 演習 `02-nginx` 完了
- Dockerfile の build 演習 `03-custom-nginx` 完了
- Docker Compose の基礎演習 `04-compose` 完了
- `k8s` vs `k3s` の違いや、DinD (Docker in Docker) などの高度なアーキテクチャ概念を Q&A に記録済み

## 現在地

Docker 基礎フェーズおよび Compose フェーズが完了し、次は「Kubernetes 基礎（Day 5）」への移行フェーズです。

次にやることは `notes/10-k8s-basics.ja.md` の読み込みと、ローカルクラスタツール `kind` を用いた環境構築です。ここで学ぶべきことは以下。

- なぜ Docker Compose では限界があり、Kubernetes が必要なのか
- `kind` (Kubernetes in Docker) がどのように DinD 技術を使ってクラスタを再現しているか
- Kubernetes ディストリビューション（`k8s`, `k3s`, `kind` など）の違い

次セッションは、利用者が `notes/10-k8s-basics.ja.md` に目を通すところ、もしくは `kind` クラスタの立ち上げ（Day 5相当）を行うところから再開してください。現状ブロックしているエラーはありません。

## まず見るファイル

1. `STATUS.ja.md`
2. `README.ja.md`
3. `REPOSITORY_POLICY.ja.md`
4. `AGENTS.md`
5. 最新の `journal/`
6. `QA.ja.md`
7. `USER_PROFILE.ja.md`
8. `COLLABORATION_GUIDE.ja.md`
9. `ASKING_GUIDE.ja.md`
10. `ENGINEER_GROWTH.ja.md`
11. `CONVERSATION_HISTORY.ja.md`

## AI への注意

利用者は AI を非常に強く信頼している。そのため、曖昧な案内や記録漏れは避けること。進捗の可視化、次アクションの明確化、質問の記録を重視すること。
