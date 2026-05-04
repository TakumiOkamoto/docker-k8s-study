# k3s 基礎

## 目的

このノートは k3s の役割と、上流 Kubernetes や他の軽量ローカルクラスタツールとの違いを説明します。

## k3s とは何か

k3s は Rancher が開発した軽量 Kubernetes ディストリビューションです。リソース制約のある環境やエッジ、ローカルの検証環境で Kubernetes を使いやすくすることを目的としています。

## なぜ k3s があるのか

- upstream Kubernetes には多くのコンポーネントやプラグイン、オプションがある。
- k3s はデフォルト設定を絞り込み、運用を簡単にする。
- 少ないメモリや低スペック環境でも高速に起動するよう設計されている。

## upstream Kubernetes との主な違い

- 単一バイナリ: `k3s` はサーバーとエージェントを一つにまとめる。
- 埋め込みデータストア: デフォルトで SQLite を使い、etcd を別途用意しない。
- 組み込みコンテナランタイム: containerd をバンドルし、デフォルトで構成する。
- オプションアドオン: Traefik、servicelb、metrics-server を自動で有効化できる。
- 依存関係を削減: 追加の etcd や CNI、外部ロードバランサが不要な場合が多い。

## それでも k3s は Kubernetes である

- k3s は Kubernetes API とリソースモデルをサポートする。
- Deployment、Service、ConfigMap、Secret などの標準マニフェストは同じように動く。
- `kubectl` で k3s クラスタを操作できる。
- 違いは主にパッケージ化、デフォルト、運用負荷であり、基本的な API セマンティクスではない。

## 典型的なユースケース

- 開発や学習環境
- CI/CD とテストクラスタ
- エッジコンピューティングや IoT
- 小規模な検証クラスタ
- upstream Kubernetes が重すぎる環境

## インストールと実行

### シンプルなインストール

```bash
curl -sfL https://get.k3s.io | sh -
```

### 動作確認

```bash
sudo k3s kubectl get nodes
sudo k3s kubectl get pods -A
```

### 代替: Docker 上で動かす k3d

- `k3d` は Docker コンテナ内で k3s クラスタを立ち上げる補助ツール。
- ワークステーション上の素早い実験に向く。

## kind との違い

- `kind` は Docker 上に upstream Kubernetes ノードを構築するツール。
- `k3s` は Kubernetes ディストリビューションそのもの。Linux ホストでも直接実行できるし、`k3d` で Docker でも動かせる。
- `kind` はノードのコンテナイメージ化に近い学習に向き、
- `k3s` は軽量 Kubernetes ディストリビューションの起動と運用を学ぶのに向いている。

## サポートエンジニアの視点

- k3s を支援するときは、基本的な Kubernetes API の理解が前提になる。
- 違いは主にデフォルトや内部コンポーネントの構成にある。
- 代表的な確認コマンド:
  - `kubectl get pods -A`
  - `kubectl describe pod <pod>`
  - `kubectl logs <pod>`
  - `sudo journalctl -u k3s`（ホスト上）
- `traefik` や `local-path-provisioner` などの k3s 固有サービスに注意する。

## トラブルシュートのヒント

- クラスタが起動しない場合は k3s サーバーログを確認する。
- Pod が Pending のままならノード状態とリソースをチェックする。
- Service が効かない場合は selector とラベル、Service タイプを確認する。
- ストレージが失敗する場合、k3s のデフォルトの local-path-provisioner を理解する。

## Kubernetes ディストリビューションとしての位置付け

- upstream Kubernetes は源流で API の標準。
- k3s は軽量化された一つのディストリビューション。
- kind は Docker 上のローカル Kubernetes クラスタ構築ツール。
- サポートでは、問題がどの層かを区別することが重要:
  - API/オブジェクトの設定
  - ノードやランタイムの環境
  - ディストリビューション固有のデフォルト
