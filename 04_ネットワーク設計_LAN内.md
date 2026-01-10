# 04 ネットワーク設計（LAN内）

## 1. 方針

* LAN内のみで運用する。
* IPは固定IPとする。
* external_url はIPアドレスを利用する。

---

## 2. IP設計

* lb-proxy: TBD
* gitlab-app: TBD
* gitlab-db: TBD
* gitlab-redis: TBD
* gitlab-gitaly: TBD
* gha-runner: TBD

---

## 3. 通信経路（疎通要件）

| From | To | Port/Proto | 用途 |
| --- | --- | --- | --- |
| LAN client | lb-proxy | 80/tcp | GitLab Web | 
| LAN client | lb-proxy | 22/tcp | Git SSH | 
| lb-proxy | gitlab-app | 80/tcp | HTTP reverse proxy | 
| lb-proxy | gitlab-app | 22/tcp | SSH L4 proxy | 
| gitlab-app | gitlab-db | 5432/tcp | PostgreSQL | 
| gitlab-app | gitlab-redis | 6379/tcp | Redis | 
| gitlab-app | gitlab-gitaly | 8075/tcp | Gitaly | 
| gha-runner | GitHub | 443/tcp outbound | GitHub Actions | 
| gha-runner | lb-proxy | 22/tcp | mirror push | 

---

## 4. 名前解決

* DNS構築は行わない。
* external_urlは `http://<lb-proxy-ip>` とする。
