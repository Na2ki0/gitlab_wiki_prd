# 10 gitlab-gitaly（Gitaly）詳細設計・手順

## 1. 目的

* Gitリポジトリ実体の保持（Gitaly専用ノード）。

---

## 2. 前提

* Gitaly専用ノードでRails系サービスは起動しない。
* `/etc/gitlab/gitlab-secrets.json` はgitlab-appと一致させる。

---

## 3. gitlab.rb 設定（確定値）

```ruby
external_url "http://<gitlab-gitaly-ip>"

# 内蔵サービス無効化
postgresql['enable'] = false
redis['enable'] = false
nginx['enable'] = false
puma['enable'] = false
sidekiq['enable'] = false
gitlab_workhorse['enable'] = false
gitlab_exporter['enable'] = false
prometheus_monitoring['enable'] = false
alertmanager['enable'] = false
node_exporter['enable'] = false
postgres_exporter['enable'] = false
redis_exporter['enable'] = false
sidekiq_exporter['enable'] = false

# DB接続抑止と内部API
gitlab_rails['auto_migrate'] = false
gitlab_rails['internal_api_url'] = 'http://<lb-proxy-ip>'

# Gitaly設定（configurationに集約）
gitaly['enable'] = true
gitaly['configuration'] = {
  'listen_addr' => '0.0.0.0:8075',
  'auth' => { 'token' => '<gitaly_token>' },
  'storage' => [
    { 'name' => 'default', 'path' => '/var/opt/gitlab/git-data/repositories' }
  ]
}
```

設定後は `gitlab-ctl reconfigure` を実行する。

---

## 4. secrets整合性手順

```bash
# gitlab-appで実行
scp /etc/gitlab/gitlab-secrets.json <user>@<gitlab-gitaly-ip>:/etc/gitlab/gitlab-secrets.json
```

```bash
# gitlab-gitalyで実行
chown root:root /etc/gitlab/gitlab-secrets.json
chmod 600 /etc/gitlab/gitlab-secrets.json
```

---

## 5. 確認

* Gitalyポート: 8075/tcpで待受
* gitlab-appから疎通可能であること
