# 07 gitlab-app（GitLab Omnibus本体）詳細設計・手順

## 1. 目的

* GitLabアプリケーション（Web/API/Sidekiq）を提供する。

---

## 2. パッケージ

* GitLab EE（Omnibus）
* 必要パッケージ: `curl`, `openssh-server`, `ca-certificates`

---

## 3. インストール

```bash
apt update
apt install -y curl openssh-server ca-certificates
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | bash
EXTERNAL_URL="http://<lb-proxy-ip>" apt install -y gitlab-ee
```

---

## 4. gitlab.rb 設定（確定値）

* 外部DB/Redis/Gitalyを使用する。
* ローカルGitalyは無効化する（`gitaly['enable'] = false`）。
* `gitlab_rails['gitaly_token']` はGitaly側と同一値にする。
* `gitlab_rails['repositories_storages']` でGitalyアドレスを指定する。

```ruby
external_url "http://<lb-proxy-ip>"

# PostgreSQL
postgresql['enable'] = false
# gitlab_rails['db_host'] = '<gitlab-db-ip>'
# gitlab_rails['db_port'] = 5432
# gitlab_rails['db_username'] = 'gitlab'
# gitlab_rails['db_password'] = '<db_password>'

# Redis
redis['enable'] = false
# gitlab_rails['redis_host'] = '<gitlab-redis-ip>'
# gitlab_rails['redis_port'] = 6379

# Gitaly (remote)
gitaly['enable'] = false
# gitlab_rails['gitaly_token'] = '<gitaly_token>'
# gitlab_rails['repositories_storages'] = {
#   'default' => { 'gitaly_address' => 'tcp://<gitlab-gitaly-ip>:8075' }
# }
```

設定後は `gitlab-ctl reconfigure` を実行する。

---

## 5. 初期設定

* 管理者アカウント: TBD
* 初期パスワード: TBD

---

## 6. 確認

* Webアクセス: `http://<lb-proxy-ip>`
* SSHアクセス: `ssh -T git@<lb-proxy-ip>`
