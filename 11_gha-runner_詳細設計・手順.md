# 11 gha-runner（GitHub Actions self-hosted runner）詳細設計・手順

## 1. 目的

* GitHub Actions self-hosted runnerとしてGitHub→GitLab mirror pushを実行する。

---

## 2. VM要件

* OS: Debian 12
* vCPU: 1
* RAM: 2GB
* Disk: 30GB

---

## 3. ネットワーク要件

* GitHubへ443/tcp（アウトバウンド）で到達可能であること。
* lb-proxyへ22/tcpで到達可能であること。

---

## 4. 必要パッケージ

* `curl`, `ca-certificates`, `git`, `openssh-client`

---

## 5. Runner登録

* 登録先: repo単位 or org単位（TBD）
* ランナー名/ラベル: TBD
* トークン管理: GitHub側で管理

---

## 6. 常駐化

* systemdサービスとして常駐させる。

---

## 7. 更新方針

* runnerのアップデート方針: TBD

---

## 8. 疎通確認

* `ssh -T git@<lb-proxy-ip>` で疎通確認
* GitHub Actionsのジョブ実行が成功すること
