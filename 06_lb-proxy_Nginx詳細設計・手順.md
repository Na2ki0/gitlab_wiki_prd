# 06 lb-proxy（Nginx：HTTP/SSH入口）詳細設計・手順

## 1. 目的

* HTTP/SSHの入口としてgitlab-appへ中継する。

---

## 2. パッケージ

* `nginx`
* `libnginx-mod-stream`

---

## 3. インストール

```bash
apt update
apt install -y nginx libnginx-mod-stream
```

* streamモジュールが読み込まれていることを前提とする。

---

## 4. 設定配置方針

* `stream {}` はトップレベルに配置する。
* HTTP設定とSSH設定は分離する。

---

## 5. 設定例

### 5.1 `/etc/nginx/nginx.conf`（トップレベル）

```nginx
stream {
  include /etc/nginx/stream.d/*.conf;
}
```

### 5.2 `/etc/nginx/stream.d/gitlab-ssh.conf`

```nginx
upstream gitlab_ssh {
  server <gitlab-app-ip>:22;
}

server {
  listen 22;
  proxy_pass gitlab_ssh;
}
```

### 5.3 `/etc/nginx/conf.d/gitlab-http.conf`

```nginx
server {
  listen 80;
  server_name _;

  location / {
    proxy_pass http://<gitlab-app-ip>;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```

---

## 6. 起動確認

```bash
nginx -t
systemctl restart nginx
systemctl status nginx
```
