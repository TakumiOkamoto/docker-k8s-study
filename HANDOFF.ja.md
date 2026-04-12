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
- `docker info` 実行済み
- `docker run hello-world` 成功済み
- `kubectl` / `kind` 利用可能
- Dockerfile 最小演習 `01-hello` 完了
- `alpine` の意味と発音に関する Q&A を記録済み
- README / notes / exercises を、サポート視点で理解できる説明に更新済み

## 現在地

次にやることは `exercises/docker/02-nginx`。ここで学ぶべきことは以下。

- ホスト側ファイルをコンテナに見せる仕組み
- ホストポートとコンテナポートの対応
- 問題発生時にホスト、コンテナ、ポート公開のどこを見るべきか

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
