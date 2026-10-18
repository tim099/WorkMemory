---
id: knowhow_readback-game-version
topic: sculpt-napoleon
title: 回讀遊戲裡匯進去的是哪一版（shipcheck 乾淨副本）
type: knowhow
status: active
created_at: 2026-10-16
created_by: kotoko
links: []
related_docs: []
---

「遊戲裡現在是哪一版」要回讀，不要聽說：ame 說 reimport 過了，可是 VoxelKit/_cache 是加密的、看不出內容。
做法：在 Blockside HEAD 的乾淨副本（git archive <sha>）跑 `python -I src/admin.py shipcheck --id napoleon`，看 reports/shipcheck_napoleon.md 的「材質」總格數與「額外動力」那一行（螺旋槳是格子／模型）。
2026-10-16 這樣回讀到 8c1ef46 匯進去的是階段十一那一版（2,133,255 格），不是階段十二（2,129,382 格）。
