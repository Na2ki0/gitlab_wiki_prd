# 12 GitHub側設定（Actionsワークフロー・Secrets・権限）

## 1. 目的

* GitHub ActionsでGitLabへmirror pushする。

---

## 2. Secrets

* `GITLAB_SSH_PRIVATE_KEY`: GitLab機械ユーザーのSSH秘密鍵
* `GITLAB_SSH_HOST`: `<lb-proxy-ip>`
* `GITLAB_SSH_PORT`: 22
* `GITLAB_REPO_URL`: `git@<lb-proxy-ip>:<namespace>/<repo>.git`

---

## 3. known_hosts

* `ssh-keyscan -p 22 <lb-proxy-ip>` で登録する。

---

## 4. ワークフロー例

`.github/workflows/gitlab-mirror.yml`

```yaml
name: Mirror to GitLab

on:
  push:
  schedule:
    - cron: '0 * * * *' # 毎時
  workflow_dispatch:

jobs:
  mirror:
    runs-on: self-hosted
    steps:
      - name: Checkout full history
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup SSH
        run: |
          mkdir -p ~/.ssh
          echo "${GITLAB_SSH_PRIVATE_KEY}" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan -p ${GITLAB_SSH_PORT} ${GITLAB_SSH_HOST} >> ~/.ssh/known_hosts
        env:
          GITLAB_SSH_PRIVATE_KEY: ${{ secrets.GITLAB_SSH_PRIVATE_KEY }}
          GITLAB_SSH_HOST: ${{ secrets.GITLAB_SSH_HOST }}
          GITLAB_SSH_PORT: ${{ secrets.GITLAB_SSH_PORT }}

      - name: Push mirror to GitLab
        run: |
          git remote add gitlab "${GITLAB_REPO_URL}"
          git push --mirror gitlab
        env:
          GITLAB_REPO_URL: ${{ secrets.GITLAB_REPO_URL }}
```

---

## 5. 展開方法

* テンプレートを各リポジトリへ配布する。
