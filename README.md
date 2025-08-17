# TCP/IP Lab

TCP/IPネットワークの学習とデバッグのためのDockerベースの実験環境です。

## 概要

このプロジェクトは、Docker Composeを使用して3つのコンテナを起動し、TCP/IPネットワークの動作を学習・観察できる環境を提供します。

- **サーバー**: Python標準のHTTPサーバーを実行
- **クライアント**: ネットワークツール一式を含むコンテナ
- **スニファー**: サーバーと同じネットワーク名前空間でパケットキャプチャが可能

## 必要な環境

- Docker
- Docker Compose

## 使い方

### 環境の起動

```bash
docker-compose up -d
```

### コンテナへの接続

クライアントコンテナに接続:
```bash
docker-compose exec client sh
```

スニファーコンテナに接続:
```bash
docker-compose exec sniffer sh
```

### HTTPサーバーへのアクセス

クライアントコンテナ内から:
```bash
curl http://server:8000
```

### パケットキャプチャ

スニファーコンテナ内で:
```bash
tcpdump -i eth0 -n
```

### 環境の停止

```bash
docker-compose down
```

## アーキテクチャ

- すべてのコンテナは `labnet` というブリッジネットワークで接続
- スニファーコンテナはサーバーと同じネットワーク名前空間を共有（`network_mode: "service:server"`）
- スニファーには `NET_ADMIN` と `NET_RAW` のケーパビリティを付与

## 提供されるWebページ

`site/index.html` にシンプルなHTMLページが含まれており、HTTPサーバーを通じて配信されます。