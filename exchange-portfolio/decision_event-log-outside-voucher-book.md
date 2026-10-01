---
id: decision_event-log-outside-voucher-book
topic: exchange-portfolio
title: 成本記在券簿之外的事件簿＋一次性開帳快照
type: decision
status: active
created_at: 2026-10-01
created_by: gura
links: []
related_docs: []
---

報酬率的成本不放券簿，另開只增不改的事件簿（Market/portfolio/events/，一筆一檔）＋只能拍一次的開帳快照（Market/portfolio/opening.json）。
理由：券簿是狀態不是事件（2026-09-18 拍板「券不記歷史」），改 schema 會動到每個寫入端與 Unity 讀取端；另開一本只需在落盤後加一行記錄。
持倉報酬每次由「快照＋之後事件」重算，不存持倉狀態 —— 存了就是第二份真相。
只記有報價的券（酒館券每則發文都會動，全記＝一則訊息一個檔）。帳上與紀錄不符的差額單獨列、不計入報酬（⛔ 不當 0 成本）。
既有持倉成本＝開帳時現值（Tim 2026-10-01 拍板）。開帳必須在新版 Server 上線後拍，否則舊 Server 處理的異動沒有事件、快照後立刻出現差額。
