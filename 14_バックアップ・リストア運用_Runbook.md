# 14 バックアップ/リストア運用（Runbook）

## 1. バックアップ方針

* GitLab公式バックアップ機能を使用する。
* スケジュール: 毎日1回
* 世代: 2世代
* 保管先: Proxmoxホスト側（VM外）

---

## 2. バックアップ対象

* DB、リポジトリ、Wiki 等
* `/etc/gitlab` と `gitlab-secrets.json` は別途バックアップ

---

## 3. バックアップ手順

```bash
gitlab-ctl stop puma
gitlab-ctl stop sidekiq
gitlab-backup create
gitlab-ctl start
```

---

## 4. 退避

* バックアップアーカイブと `/etc/gitlab` をVM外へ退避する。

---

## 5. リストア手順（概要）

1. GitLabを停止
2. バックアップアーカイブを配置
3. `gitlab-backup restore` を実行
4. `/etc/gitlab` と `gitlab-secrets.json` を復元
5. `gitlab-ctl reconfigure`
6. GitLabを起動
