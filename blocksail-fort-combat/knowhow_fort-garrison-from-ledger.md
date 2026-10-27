---
id: knowhow_fort-garrison-from-ledger
topic: blocksail-fort-combat
title: 換上來的堡壘駐軍照城鎮帳那一棟今天幾個兵；手改帳測法
type: knowhow
status: active
created_at: 2026-10-27
created_by: summit
links: [blocksail-fort-combat/knowhow_town-fort-wake]
related_docs: []
---

城裡的堡壘換上來時的駐軍（Blockside 42f529d，TASK-0955，2026-10-27）：
- 63_towns.js townFortWake 讀 Blocksail `live.work[地塊]`（globe.html townLive 的 work ＝ 帳的 staff；BSW.GARRISON 分到每一棟，TASK-0948）⇒ 40_crew.js `crewCap(sh, n)`：多的人標 preOut（st 3），crew0 ＝ 留下的人數。帳上 0 ⇒ struck「沒有駐軍」；帳上沒有那一棟 ⇒ 設定檔滿編。
- 換上來的那一棟，townWorkPrep 跳過（站崗鎮民不畫）；換的時候 `T.work = null` 讓它重擺。
- 測法：`__globe.clock(24*d+9)` 讓帳過幾天 ⇒ `__globe.ledger().towns[城].staff[地塊] = n` 手改（同一份帳物件，battleIn 時 townLive 讀得到）⇒ battleIn('sail') ⇒ iframe 加 &debug=1、post go、截幾張小圖讓城建出來 ⇒ `__game.townFort('<地塊>')` 換 ⇒ `__game.ships()` 是原始船物件（能直接看 crew、gunsManned、fate）。
- 皇家港的四座（fort-charles en-fort、fort-james／carlisle／rupert en-battery）都換得了；英國稜堡設定檔軍官 2 位一位都沒站上（改之前就這樣）。
