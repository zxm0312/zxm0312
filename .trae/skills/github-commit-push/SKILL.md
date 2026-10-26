---
name: "github-commit-push"
description: "提交项目到 GitHub——当用户要求'提交项目到 GitHub'、'push 到 GitHub'、'commit 并 push 到 zxm0312'时使用。通过 SSH 推送到 git@github.com:zxm0312/zxm0312.git。"
---

# 提交项目到 GitHub（git commit + push）

将当前项目的所有改动用 git 提交，并通过 **SSH 公钥认证**推送到 GitHub 仓库。

## 远程仓库信息

- SSH URL：`git@github.com:zxm0312/zxm0312.git`
- HTTPS URL（备用）：`https://github.com/zxm0312/zxm0312.git`
- 建议 remote 名称：`github`（保留 origin 指向原 Gitee 仓库时）
- 若只有 GitHub 一个远程，可直接覆盖 `origin`

## 前置条件

- **SSH 公钥已配置**：运行 `ssh -T git@github.com` 测试连接
- 若未配置，引导用户：
  1. `ssh-keygen -t ed25519 -C "your_email@example.com"`
  2. `cat ~/.ssh/id_ed25519.pub` → 复制到 GitHub Settings → SSH keys

## 执行步骤

按顺序执行，每步成功后再进行下一步：

1. **追加提示词**（项目规则 zhang_prompt.md）：
   - 在执行任何 git 操作前，**先**将本次用户提示词追加写入 `zhang_prompt.md`
   - 格式：`## yyyy-MM-dd-HH-mm` + `- 模型: Trae (专有模型)` + `- 提示词:` + 原文

2. **检查状态**：
   ```bash
   git status --short
   git diff --stat
   ```
   了解待提交的改动。

3. **配置 remote**：
   - 先看现有配置：`git remote -v`
   - 新增 GitHub 远程（推荐，保留 origin）：
     ```bash
     git remote add github git@github.com:zxm0312/zxm0312.git
     ```
   - 若要直接替换 origin：
     ```bash
     git remote set-url origin git@github.com:zxm0312/zxm0312.git
     ```

4. **SSH 连通性测试**（首次）：
   ```bash
   ssh -T git@github.com
   ```
   成功返回 "Hi zxm0312!" 即可继续；失败则停下来协助用户配置公钥。

5. **暂存**：
   ```bash
   git add -A
   ```
   或排除敏感文件：`git add . ':!*.key' ':!.env'`

6. **提交**：
   ```bash
   git commit -m "<中文描述>"
   ```
   - 简述本次改动内容（例如 `添加第14章数7游戏页面`、`update index links`）
   - 若用户附带提交说明，直接使用
   - 若报 "nothing to commit"，告知用户并终止

7. **确定分支名**：
   ```bash
   git branch --show-current
   ```

8. **推送**：
   - 首次推送新远程：`git push -u github <branch>`
   - 后续推送：`git push github <branch>`
   - 若远程有本地没有的提交导致被拒绝：`git pull --rebase github <branch>` 解决后再推送
   - 有冲突时停下来向用户报告，**不要 force push**

9. **确认结果**：展示 `git log --oneline -3` 与 push 输出，确认推送成功。

## 注意事项

- **不要使用 `git push --force`**
- 不要提交密钥、密码等敏感文件；如发现加入 `.gitignore`
- GitHub SSH 认证失败时，引导用户确认公钥是否已添加到 GitHub 账户
- 本项目同时有 Gitee（origin）和 GitHub（github）两个远程时，两个都要 push 请明确告知用户

## 本项目特定约定

- 所有 CSS 类名 / ID / JS 变量以 `zhang_` 开头
- 新建 HTML 页面后需在 `index.html` 添加对应链接（见 `agents.md`）
- 执行任何命令前先将提示词追加写入 `zhang_prompt.md`
