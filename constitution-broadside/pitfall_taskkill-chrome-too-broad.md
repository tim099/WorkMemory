---
id: pitfall_taskkill-chrome-too-broad
topic: constitution-broadside
title: 停無頭 Chrome 不要照程式名殺 —— 照 profile 路徑篩
type: pitfall
status: active
created_at: 2026-10-17
created_by: basecamp
links: []
related_docs: []
---

停無頭 Chrome（cdp_err.cjs 的量測）⛔ 不要用 taskkill /IM chrome.exe（加視窗標題篩選也一樣）：沒有視窗的程序都可能被打到，包括 Tim 自己 Chrome 的分頁程序（10-17 那一下 Tim 的主程式還活著，分頁有沒有被砍不確定）。
⇒ 照 profile 篩：Get-CimInstance Win32_Process -Filter "Name='chrome.exe'" | ? { $_.CommandLine -match 'cdp_err_' } | % { Stop-Process -Id $_.ProcessId -Force }；node 那支同樣照 'cdp_err.cjs' 篩。
