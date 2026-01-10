# 02 Proxmox / VM払い出し設計・手順

## 1. 目的

* 6VM構成をProxmox上で作成する。

---

## 2. VM一覧とリソース

| VM | vCPU | RAM | Disk | OS |
| --- | --- | --- | --- | --- |
| lb-proxy | 1 | 1GB | 8GB | Debian 12 |
| gitlab-app | 4 | 8GB | 50GB | Debian 12 |
| gitlab-db | 2 | 4GB | 50GB | Debian 12 |
| gitlab-redis | 1 | 2GB | 10GB | Debian 12 |
| gitlab-gitaly | 2 | 8GB | 200GB | Debian 12 |
| gha-runner | 1 | 2GB | 30GB | Debian 12 |

合計目安: 11 vCPU / 25GB RAM / 350GB Disk

---

## 3. VM作成方針

* NIC: 1枚（LAN内ブリッジ）
* IP: 固定IP（具体値はTBD）
* ディスク: 必要に応じて後で拡張可能なプロビジョニング
* スナップショット: 重要な変更前に取得

---

## 4. バックアップ方針（Proxmox側）

* GitLabバックアップはVM外退避を前提とする。
* Proxmoxバックアップは補助的に利用する（TBD）。

---

## 5. 作成手順（概要）

1. Debian 12 ISOを登録
2. VMを作成（vCPU/RAM/Disk/NIC指定）
3. OSインストール
4. 固定IP/ホスト名設定
5. スナップショット取得
