---
name: tree
description: 生成项目目录树结构。当用户说"查看目录结构"、"生成树"、"tree"或类似请求时使用。
---

# tree 目录树生成

生成当前项目的目录结构树状图，方便了解文件组织。

## Workflow

1. 确认当前工作目录（`pwd`），从项目根目录开始。
2. 检查系统是否有 `tree` 命令：
   - Windows：`tree /F`（显示文件）或 `tree`（仅目录）。
   - Git Bash / WSL：`tree -L <深度>`（需先安装）。
3. 若系统无 `tree` 命令，使用 PowerShell 脚本替代（见下方）。
4. 运行命令并将输出以代码块形式展示给用户。
5. 可选择性地将结果保存为 `tree.txt` 或在用户请求时更新到 `AGENTS.md` 的结构部分。

## PowerShell 替代方案

当系统没有 `tree` 命令时，在 PowerShell 中执行：

```powershell
function Get-Tree($Path = ".", $Depth = 3, $Prefix = "") {
    Get-ChildItem -LiteralPath $Path -Force | Where-Object {
        $_.Name -notin @(".git", "node_modules", ".opencode") -and (
            $_.FullName -eq (Resolve-Path $Path).Path -or
            ($_.FullName.Length - (Resolve-Path $Path).Path.Length) -le $Depth * 4
        )
    } | ForEach-Object {
        $indent = "$Prefix├── "
        if ($_.PSIsContainer) {
            Write-Output "$indent$($_.Name)/"
            Get-Tree -Path $_.FullName -Depth $Depth -Prefix "$Prefix│   "
        } else {
            Write-Output "$indent$($_.Name)"
        }
    }
}
Get-Tree -Depth 4
```

或者更简洁的一行式（输出到文件）：

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
tree /F | Out-File -Encoding UTF8 tree.txt
```

## 深度建议

- 项目较浅（如本站）：深度 2–3 足够。
- 包含多级子目录的项目：深度 4–5。
- 用户未指定时默认 3 层。

## 忽略规则

生成树时默认忽略以下目录（如存在）：
- `.git/`
- `.opencode/`
- `node_modules/`
- 其他临时/缓存目录

## 注意事项

- PowerShell 默认输出可能导致中文乱码，先执行 `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8`。
- 使用 `tree /F` 时会包含所有文件，注意输出长度；可用 `tree /F | more` 分页。
- 不要覆盖 `AGENTS.md` 中的结构部分，除非用户明确要求更新。
