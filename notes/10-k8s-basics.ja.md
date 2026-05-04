# Kubernetes 基礎

## 目的

このノートは Kubernetes をコンテナ化されたワークロードの制御面として理解することを目的としています。単なるコマンド集ではありません。

## なぜ Kubernetes なのか？

Docker Compose はローカルの複数コンテナ構成に便利ですが、望ましい状態を継続的に管理する仕組みは持ちません。Kubernetes は次の課題を解決します。

- アプリレプリカが落ちたときの自動復旧
- Pod 再起動後に変わる IP アドレス
- 動的な Pod 群へのトラフィックルーティング
- 制御面とコンテナランタイムの責務分離
- ノード間の一貫したスケーリング

## コア概念

- **Cluster**: 複数のノードをまとめて管理する単位
- **Node**: Pod を実行するマシン
- **Pod**: 最小の実行単位。1つ以上のコンテナを持つ
- **Deployment**: ReplicaSet と Pod を宣言的に管理する制御機構
- **Service**: Pod 群への安定したネットワークエンドポイント
- **Namespace**: クラスタ内の論理的な分離
- **ConfigMap**: 機密でない設定データ
- **Secret**: 機密データ

## Kubernetes のアーキテクチャ

Kubernetes は制御プレーンとデータプレーンに分かれます。

### 制御プレーン

- **API サーバー**: すべてのリクエストの入口
- **etcd**: クラスタ状態の永続ストレージ
- **スケジューラー**: Pod をノードに割り当てる
- **コントローラーマネージャー**: Deployment や Service などの望ましい状態を維持する

### データプレーン

- **kubelet**: 各ノード上で Pod とコンテナを管理するエージェント
- **コンテナランタイム**: コンテナイメージを実行するソフトウェア
- **kube-proxy**: 各ノード上で Service ネットワーキングを維持する

## Kubernetes の動き方

Kubernetes は宣言的です。

- マニフェストに望ましい状態を記述する
- `kubectl apply` で適用する
- 制御プレーンが望ましい状態と実際の状態を比較する
- コントローラーが実際の状態を望ましい状態に近づけるために行動する

このループを「リコンシリエーション」と呼びます。

## 主要リソース型

### Pod

Pod は最小の実行単位です。ネットワークとストレージを共有する 1 つ以上のコンテナを含みます。

### ReplicaSet / Deployment

Deployment は ReplicaSet を管理し、ReplicaSet は Pod を管理します。
Deployment は指定されたレプリカ数を維持します。

### Service

Service は Pod への安定した DNS 名と IP を提供します。
Pod は入れ替わりがあっても、Service の到達点は変わりません。

### Namespace

Namespace はチームや環境を分離します。
リソースのスコープを分け、名前衝突を防ぎます。

### ConfigMap / Secret

ConfigMap と Secret は Pod に設定を渡すために使います。
Secret は機密データのための専用リソースです。

## kind でのローカル学習

`kind` は Kubernetes IN Docker の略です。
Docker ホスト上に Kubernetes ノードコンテナを作成します。

### kind がなぜ便利か

- クラスタを素早く作成・削除できる
- upstream Kubernetes 画像を使う
- クラスタ構成の学習に適している
- 別の VM を必要としない

### kind が kubeconfig に追加するもの

`kind create cluster --name study` を実行すると、kind は `~/.kube/config` に以下を追加します。

- **cluster** エントリ: API サーバーのエンドポイントと証明書
- **user** エントリ: クライアント認証情報
- **context** エントリ: cluster と user を組み合わせた接続設定

この context は通常 `kind-<cluster-name>` になります。

## Kubernetes ディストリビューション

Kubernetes は API の標準であり、リファレンス実装でもあります。
多くのプロダクトやツールがそれを異なる形でパッケージ化しています。

### Upstream Kubernetes (`k8s`)

- ソースプロジェクトであり API の標準
- フルセットのコンポーネントと制御プレーン機能を含む

### kind

- upstream Kubernetes を Docker コンテナ上で実行するローカルクラスタツール
- クラスタトポロジーと Kubernetes 挙動の学習に便利

### k3s

- 小規模やエッジ環境向けに最適化された軽量ディストリビューション
- よりシンプルなパッケージ化と依存関係の削減
- 標準 Kubernetes API をサポートする

## 最初に見るコマンド

```bash
kubectl get nodes
kubectl get pods -A
kubectl get deployments -A
kubectl get services -A
kubectl get namespaces
kubectl get events -A
```

## デバッグの流れ

1. `kubectl get pods -A`
2. `kubectl describe pod <pod-name>`
3. `kubectl logs <pod-name>`
4. `kubectl get events -A`

## サポートチェックリスト

- Pod は正しい namespace にいるか？
- labels と selector は一致しているか？
- Deployment の replica 数は正しいか？
- Service の type や port 設定は適切か？
- CrashLoopBackOff やスケジューリング失敗があるか？

## 初期によくある失敗

- Pod を直接編集してしまう
- labels / selectors を忘れる
- `ContainerCreating` を失敗と判断してしまう
- Deployment を Service で公開するのを忘れる
- Pod IP が変わることを期待してしまう

## 次の学習ステップ

- `exercises/k8s/01-deployment/deployment.yaml` を apply する
- Service のルーティングと Pod ラベルを確認する
- Deployment をスケールして挙動を観察する
- `kind` と `k3s` を比較してディストリビューションの違いを理解する
