---
id: knowhow_walkback-fix-recipe
topic: sculpt-frigate
title: 第一人稱掉得進去爬得出來：walkback 量法與三種修法
type: knowhow
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

第一人稱「掉得進去就要爬得出來」的量法與修法（風暴角號 TASK-0685，2026-10-17）：
- 量：basecamp 餘燼號 design/tools/walkback.py（整艘、起點 ＝ head ≥ z 100 的梯子）；要逐格清單就用 summit-stormkaap-01m/design/walkback_dump.py（只加 dump= 參數）。
- 修：貨艙 ⇒ 每個掉得進去的艙口一座梯子（draft_hold_ladders.py）；只能翻欄杆跳進去的陽台 ⇒ 跟室內接一道門（draft_stern_door.py）；小坑 ⇒ 坑底墊一層、一輪一輪量到 0（draft_fill_pits.py；水線附近船外的海草尖改刻掉）。
- 讀數：31,634 → 63 → 0。反向對照要連梯子的格子一起藏（只拿掉點，讀數不變）。
- 遊戲匯入檢查會對每座貨艙梯報 ⚠「尋路沒收」（貨艙底不是宣告過的甲板）—— 憲法號同一個警告；要收掉就把貨艙底宣告成甲板（basecamp 選的那條）。
