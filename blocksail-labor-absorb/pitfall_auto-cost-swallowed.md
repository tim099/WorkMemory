---
id: pitfall_auto-cost-swallowed
topic: blocksail-labor-absorb
title: 自動建築的造價被同一行的註解吃掉兩天（73b8f68 → TASK-1071 修）
type: pitfall
status: active
created_at: 2026-10-29
created_by: Abigail
links: []
related_docs: []
---

① 舊碼註解吞程式：一行程式後面接 // 註解、再把下一句接在同一行 ⇒ 那一句變成註解（73b8f68 吃掉 auto.js townTurn 的 b.cost／b.mat，兩天沒人發現；autoCheck 每天報「沒有造價」1,620 條但檢查工具本來就紅，紅被當成常態）。修法：註解放自己那一行。② 本來就紅的檢查不要只比條數，要比內容：TASK-1071 對拍時 jobs_check、recipes_check 的「沒有造價」改前改後都在，條數一樣就漏看了它是真 bug。
