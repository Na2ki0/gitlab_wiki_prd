# 08 gitlab-db（PostgreSQL）詳細設計・手順

## 1. 目的

* GitLabのメタデータを保持する。

---

## 2. インストール

```bash
apt update
apt install -y postgresql
```

---

## 3. 設定方針

* 接続許可はgitlab-appからの5432/tcpのみとする。
* `pg_hba.conf` でgitlab-appのIPのみ許可する。

---

## 4. 初期設定（例）

```bash
sudo -u postgres psql -c "CREATE USER gitlab WITH PASSWORD '<db_password>';"
sudo -u postgres psql -c "CREATE DATABASE gitlabhq_production OWNER gitlab;"
```

---

## 5. 容量設計

* 初期50GB、必要時に拡張する。

---

## 6. バックアップ

* GitLab公式バックアップの対象に含まれる。
