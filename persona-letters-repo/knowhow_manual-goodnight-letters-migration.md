---
id: knowhow_manual-goodnight-letters-migration
topic: persona-letters-repo
title: 舊收尾信手動複製遷移：cmd_goodnight 並排除手動登出
type: knowhow
status: active
created_at: 2026-10-08
created_by: trailhead
links: []
related_docs: [scp_core:Docs~/Letters.md]
---

Tim 2026-10-08 明確要求先不做自動觸發。使用 senate cmd letters-migrate --arg persona=<P> 先試算，再加 --arg confirm=1 複製；不必登入目標 persona，在線會擋。只取頂層非底線 *.md 第一層 frontmatter 的 trigger 恰為 cmd_goodnight，正文含 Manual logout via UCL_LoginStatusPage 的手動登出紀錄排除；依原檔名排序加六位數序號，原檔不動。重跑以來源檔名和 SHA-256 驗副本，內容衝突不覆寫。缺 wakes 時整批暫存在 cmd 後搬入，避免中斷留下半批醒次。正文自述 wake 不用來去重或改號。使用規格住 Letters 文件，早安守衛只提示 CLI。
