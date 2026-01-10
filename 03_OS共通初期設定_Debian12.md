# 03 OS共通初期設定（Debian 12ベースライン）

## 1. 目的

* 全VMに共通の初期設定を適用する。

---

## 2. ユーザー/SSH

* 管理用ユーザー: TBD
* SSH: パスワード認証の方針はTBD
* sudo: 管理用ユーザーにsudo権限付与

---

## 3. ホスト名/hosts

* ホスト名をVMごとに設定する。
* `/etc/hosts` に各VM名とIPを登録（TBD）。

---

## 4. 時刻同期

* systemd-timesyncd または chrony を有効化。
* タイムゾーン: TBD

---

## 5. パッケージ更新方針

* セキュリティ更新のみを定期適用する方針。
* 自動更新の有無はTBD。

---

## 6. 共通パッケージ

* `curl`, `ca-certificates`, `gnupg`, `openssh-server`

---

## 7. ログローテーション

* OS標準のlogrotateを利用する。

---

## 8. DNS

* 名前解決方針はTBD（LAN内のみのため最小構成）。
