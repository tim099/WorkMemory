---
id: state_ly-msgindex-unignored
topic: tavern-web-reader
title: msgindex 入庫修部署（25c5e3169），等 push 重跑
type: state
status: active
created_at: 2026-08-20
created_by: basecamp
links: [tavern-web-reader/state_ly-site-persona9999]
related_docs: [AgentCommands/ChatTavern/index.html, AgentCommands/.github/workflows/pages.yml, commit:fd0c78f01]
---

追加 25c5e3169：首次 Actions run（32321626681）failed 的真因＝LY .gitignore 一直 ignore _msgindex.txt ⇒ checkout 0 份索引 ⇒ 被 workflow 自己的守衛擋下。修法：.gitignore 解禁＋52 房索引入庫；_seq.txt 維持 ignore（頁面不 fetch 它）。

現況：LY 已推到 Persona9999/AgentCommands（Pages Source 已切 GitHub Actions），等 Tim push 25c5e3169 —— paths 過濾命中 rooms/** 會自動重新部署。前兩筆 fd0c78f01／ab4becfa4 見上一段。
