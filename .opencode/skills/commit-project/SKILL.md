---
name: commit-project
description: 提交项目到 Gitee——当用户要求"提交项目"、"提交代码"、"commit 并 push"、"推送到 gitee"、"上传到 gitee"时使用。执行 git commit 并 push 当前项目到 https://gitee.com/jiushun/zxm0312.git。
---

# 提交项目（git commit + push 到 Gitee）

将当前项目的所有改动用 git 提交，并推送到远程仓库 `https://gitee.com/jiushun/zxm0312.git`。

## 远程仓库信息

- URL：`https://gitee.com/jiushun/zxm0312.git`
- remote 名称：`origin`

## 执行步骤

按顺序执行，每步成功后再进行下一步：

1. **检查状态**：运行 `git status --short` 和 `git diff --stat`，了解待提交的改动。
2. **配置 remote**（仅当缺失时）：
   - `git remote` 输出中没有 `origin` 时执行 `git remote add origin https://gitee.com/jiushun/zxm0312.git`
   - 已有 `origin` 但 URL 不同，先问用户是否改为上述 URL，再执行 `git remote set-url origin https://gitee.com/jiushun/zxm0312.git`
3. **暂存**：`git add -A`
4. **提交**：`git commit -m "<message>"`
   - message 用中文或英文简述本次改动内容（例如 `添加第5章页面`、`update index links`）。
   - 若用户在调用时附带了提交说明，直接使用用户的说明作为 message。
   - 若报 "nothing to committed"，说明没有改动，直接告知用户并终止。
5. **确定分支名**：`git branch --show-current`（本项目初始分支为 `master`）。
6. **推送**：
   - 仓库尚无任何提交（首次推送）：`git push -u origin <branch>`
   - 已有提交：`git push origin <branch>`（若远程分支不存在，用 `git push -u origin <branch>`）
   - 若远程有本地没有的提交导致 push 被拒绝，先执行 `git pull --rebase origin <branch>` 解决后再推送；有冲突时停下来向用户报告，不要强行 force push。
7. **确认结果**：展示 `git log --oneline -3` 与 push 输出，确认推送成功。

## 注意事项

- 不要使用 `git push --force`。
- 不要提交密钥、密码等敏感文件；如果发现疑似敏感文件（如 `*.key`、`.env` 等），先提醒用户并询问是否排除（可加入 `.gitignore`）。
- 遇到 Gitee 认证失败（`Authentication failed` / `403`）时，提示用户配置凭据：可执行 `git config --global credential.helper store` 后重新推送，由用户输入 Gitee 用户名和密码/令牌。
