---
name: push-github
description: 使用 git commit 并 push 当前项目到 GitHub 仓库 ssh://git@ssh.github.com:443/15704195299/cuiyiwen.git 。当用户说"提交到GitHub"、"推送到GitHub"、"push到github"或类似请求时使用。
---

# 推送到 GitHub

将当前项目的更改提交（commit）并推送（push）到 GitHub 远程仓库。

Gitee 远程 `origin` 保持不变，GitHub 远程名为 `github`。

远程地址（SSH over 443，绕过 22 端口封锁）：`ssh://git@ssh.github.com:443/15704195299/cuiyiwen.git`

## 背景说明

本机 hosts 被 Steam++ 劫持（`github.com → 127.0.0.1`），且 22 端口不可达。
解决方案：使用 GitHub 的 **SSH over port 443**，通过 `ssh.github.com:443` 连接（该域名未被 hosts 劫持）。

## Workflow

1. 确认当前目录是 git 仓库（`git rev-parse --is-inside-work-tree`）。
2. 检查远程配置（`git remote -v`）：
   - 若不存在名为 `github` 的远程，执行：
     ```
     git remote add github ssh://git@ssh.github.com:443/15704195299/cuiyiwen.git
     ```
   - 若 `github` 已存在但地址不同，执行：
     ```
     git remote set-url github ssh://git@ssh.github.com:443/15704195299/cuiyiwen.git
     ```
   - `origin`（Gitee）保持不变，不要修改。
3. 首次使用前（或 known_hosts 被清除后），确保主机密钥已信任：
   ```
   ssh -T -p 443 -o StrictHostKeyChecking=accept-new git@ssh.github.com
   ```
   应看到 `Hi 15704195299! You've successfully authenticated`。
4. 运行 `git add -A` 暂存全部更改。
5. 检查暂存区（`git status --short`）：
   - 若没有任何更改，告知用户"没有需要提交的更改"并停止。
   - 若暂存内容包含疑似敏感文件（密码、密钥、私钥等），先提醒用户确认。
6. 生成提交信息并执行 commit：
   - 提交信息使用中文，简要概括本次更改内容。
   - 若用户提供了提交信息，优先使用。
   - 命令：`git commit -m "<提交信息>"`。
7. 确定当前分支（`git branch --show-current`），默认为 `master`。
8. 推送到 GitHub：
   - 首次推送：`git push -u github <分支名>`。
   - 后续推送：`git push github <分支名>`。
9. 推送失败时的常见处理：
   - **Host key verification failed**：重新执行第 3 步写入 known_hosts。
   - **SSH 认证失败**：提醒用户确认公钥是否已添加到 GitHub 账户（Settings → SSH and GPG keys）。
   - **non-fast-forward**：先 `git pull --rebase github <分支名>`，解决冲突后再推送。
   - **仓库为空**：GitHub 空仓库首次推送直接用 `git push -u github <分支名>` 即可。
   - **22 端口拒绝**：确认 remote URL 是 `ssh://git@ssh.github.com:443/...` 格式。
10. 推送成功后，向用户报告提交哈希和仓库地址 https://github.com/15704195299/cuiyiwen 。

## 注意事项

- 本仓库文件名包含中文和空格，PowerShell 下先执行 `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8`。
- 不要使用 `git push --force`，除非用户明确要求。
- 不要跳过第 5 步的敏感文件检查。
- 不要修改 Gitee 的 `origin` 远程，GitHub 用独立的 `github` 远程。
- 本项目 GitHub 远程**必须**使用 `ssh://git@ssh.github.com:443/...` 格式，不可用 `git@github.com:...`（hosts 劫持 + 22 端口封锁）。
