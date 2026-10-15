---
name: 提交项目
description: 使用 git commit 并 push 当前项目到 Gitee 远程仓库 https://gitee.com/cui-1yiwen/cuiyiwen2026。当用户说"提交项目"、"推送到 Gitee"、"上传到码云"或类似请求时使用。
---

# 提交项目

将当前项目的全部更改提交（commit）并推送（push）到 Gitee 远程仓库。

远程地址：`https://gitee.com/cui-1yiwen/cuiyiwen2026`

## Workflow

1. 确认当前目录是 git 仓库（`git rev-parse --is-inside-work-tree`），若不是则先 `git init`。
2. 检查远程配置（`git remote -v`）：
   - 若不存在名为 `origin` 的远程，执行 `git remote add origin https://gitee.com/cui-1yiwen/cuiyiwen2026.git`。
   - 若 `origin` 存在但地址不同，先与用户确认再修改。
3. 运行 `git add -A` 暂存全部更改（包括新文件、修改和删除）。
4. 检查暂存区（`git status --short`）：
   - 若没有任何更改，告知用户"没有需要提交的更改"并停止。
   - 若暂存内容包含疑似敏感文件（密码、密钥、私钥等），先提醒用户确认。
5. 生成提交信息并执行 commit：
   - 提交信息使用中文，简要概括本次更改内容，例如 `提交网页练习页面`。
   - 若用户在调用时提供了提交信息，优先使用用户提供的。
   - 命令：`git commit -m "<提交信息>"`。
6. 确定当前分支（`git branch --show-current`），默认为 `master`。
7. 推送到远程：
   - 首次推送（远程还没有该分支）：`git push -u origin <分支名>`。
   - 后续推送：`git push origin <分支名>`。
8. 推送失败时的常见处理：
   - **认证失败**：提示用户配置 Gitee 凭据（Git Credential Manager 或 SSH 密钥），不要在命令行中硬编码用户名/密码。
   - **non-fast-forward（远程有新提交）**：先 `git pull --rebase origin <分支名>`，解决冲突后再推送；有冲突时向用户报告冲突文件，不要强行覆盖。
   - **仓库为空**：Gitee 空仓库首次推送用 `git push -u origin <分支名>` 即可。
9. 推送成功后，向用户报告提交哈希和远程仓库地址 `https://gitee.com/cui-1yiwen/cuiyiwen2026`。

## 注意事项

- 本仓库文件名包含中文和空格，git 命令本身不受影响，但展示 `git status` 输出时注意 PowerShell 的编码问题（可先设置 `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8`）。
- 不要使用 `git push --force`，除非用户明确要求。
- 不要跳过第 4 步的敏感文件检查。
