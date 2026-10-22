# AGENTS.md

## What this repo is
Flat static HTML/CSS/JavaScript practice site. No package manager, build, lint, typecheck, tests, or CI — there is nothing to install or run. All code is vanilla HTML/CSS/JavaScript (no frameworks, no third-party libraries, no CDN). Icons use local iconfont files from `font/`. Verification = open the changed file in a browser; relative links/images only resolve from this directory.

## Structure
- `1-1.html` — course portfolio homepage ("122崔溢文的《高级网页设计》课堂实践"); links to every exercise page. This is the "首页" that new pages must link back to.
- `index.html` — "好书推荐" book-recommendation page; links to `b1.html`–`b12.html`.
- `b1.html`–`b12.html` — book detail pages; each links back via `./index.html`; covers are `img/b1.jpg`–`img/b12.jpg`.
- Numbered files `2-1.html` … `11-4.html` — standalone CSS course exercises (some have Chinese filenames, e.g. `5-1 层叠与继承.html`). Self-contained except:
  - `5-1 层叠与继承.html` is the **only** file that links `css.css`.
  - `2-2.html` links to `2-1.html`.
- `网页元素1原图实现.html`–`网页元素3原图实现.html` — pixel-recreation exercises.
- `img/` — shared images (`1.jpg`–`3.jpg`, `a1`–`a3.jpg`, `b1`–`b12.jpg`). All references are relative (`./img/...`); keep new assets here.

## Conventions
- All files are UTF-8 with `<meta charset="UTF-8">`; preserve that when creating or editing pages.
- Filenames contain spaces and Chinese characters — quote paths in shell commands: `start "" "5-1 层叠与继承.html"`.
- PowerShell prints non-ASCII filenames as mojibake by default. Run `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8` first (or use the glob/grep tools) to see real names.
- Don't add a shared stylesheet or refactor inline CSS into files — the one-file/one-page style is deliberate.

## Project rules (user-mandated, always apply)
- **Naming**: every CSS class name, id name, and JS variable must start with `cyw_` (e.g. `.cyw-box`, `#cyw-nav`, `let cyw_count`). Applies to all files — new and existing.
- **HTML header comment**: every HTML file must include a comment at the top of `<head>` in the format:
  ```html
  <!-- 姓名：崔溢文，日期：YYYY-MM-DD，功能名称：XXX -->
  ```
  Use the file's creation or last significant edit date.
- **Stack**: vanilla HTML + CSS + JavaScript only. No frameworks, no third-party libraries, no external CDN links. Iconography uses local iconfont files in `font/`.
- **Data fetching**: use the browser `fetch` API for any server-side data; no AJAX libraries, no axios, no jQuery.
- **Prompt log**: before executing any user prompt, append an entry to `cui_prompt.md` (append only; never overwrite previous entries). Each entry must contain:
  1. heading line with timestamp formatted `yyyy-mm-dd-hh-mm`
  2. the model used (e.g. `opencode/mimo`)
  3. the full prompt text
- **Homepage link**: every HTML file must include a hyperlink back to the homepage `1-1.html` (e.g. a footer nav link `<a href="./1-1.html">首页</a>`).
