# Repository Policy

この repository は Docker と Kubernetes の学習を継続するための個人用 private repository。

## 目的

- 学習の現在地を常に明確にする
- その日の作業内容を後から再現できるようにする
- AI と協働して、迷わず次の一歩に進めるようにする
- 将来 Docker や Kubernetes をサポートする立場で説明できる理解を作る

## ドキュメント運用

- 日本語文書は `*.ja.md` とする
- 主要な概念説明は英語版と日本語版の両方を持つ
- 現在地の要約は `STATUS.ja.md` に置く
- 日々の詳細は `journal/YYYY-MM-DD.md` に置く
- トピック別メモは `notes/` に置く
- 実行可能な最小サンプルは `exercises/` に置く
- 設定ファイルや manifest の意味を Markdown で説明する

## 進捗の見え方

この 3 つを見れば迷わない状態を保つ。

1. `README.ja.md`
2. `STATUS.ja.md`
3. 直近の `journal/YYYY-MM-DD.md`

## Commit Policy

- commit message は必ず Conventional Commits に従う
- commit message は英語で書く
- 1 commit 1 意図を基本にする

### 例

- `docs: update nginx exercise progress`
- `feat: add compose starter example`
- `fix: correct kind setup note`
- `chore: initialize study repository`

## AI 協働ポリシー

- AI は良き学習パートナーとして、次に何をやるべきかを明確に示す
- AI は進捗メモ、要約、補助ファイル更新を積極的に行う
- AI は git push 直前までの作業を担当する
- 利用者は必要に応じてローカル実行結果を AI に共有する
