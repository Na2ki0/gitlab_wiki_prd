# 09 gitlab-redis（Redis）詳細設計・手順

## 1. 目的

* GitLabのキャッシュ/ジョブ管理用途。

---

## 2. インストール

```bash
apt update
apt install -y redis-server
```

---

## 3. 設定方針

* 接続許可はgitlab-appからの6379/tcpのみとする。
* bindアドレスはgitlab-redisのIPに限定する。
* 永続化方針: TBD

---

## 4. バックアップ

* Redisはバックアップ対象外。
