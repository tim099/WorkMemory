---
id: decision_fort-one-at-a-time-swapback
topic: blocksail-fort-combat
title: 城裡的堡壘：點了才換、一次一座、離開換回；收船要補序號
type: decision
status: active
created_at: 2026-10-27
created_by: unknown
links: []
related_docs: []
---

TASK-0955 結單（2026-10-27）時定下來的。Tim 原話：「維持點了換上來吧 且一次一座（之後要做攻城時再處理相關機制 還要想）」「第 3 格改成『開火就敵對』，勢力關係等外交系統」。

- **換上來**：航海時點城裡的堡壘、砲台，才換成戰鬥單位，一次一座；點下一座，前一座就換回城裡的模型。射程內自己醒、好幾座一起打，留給攻城再想。
- **換回去**（63_towns.js townFortSleep／townFortTick）：
  - 什麼時候換回：離開時（射程 ×1.25，而且比換上來那時遠 300 m 以上）、換玩法、點了另一座。
  - 它的砲彈還在飛 ⇒ 標 townRetire，等落地才收；等的那幾秒 calm 停手、被打也不翻臉。
  - 打壞的格子、死傷都不留（那是 0956）。
- **敵我**：開火就敵對。照勢力分敵我要等外交系統。
- **砲彈**：只撞船和地形（bedHgt）。城裡沒換上來的建築都是畫面，照穿過。
- 🩸 **收船要補序號**：從 SHIPS 拿掉一艘時，記序號的都要跟著往前補 —— 砲彈 b.owner、黏在船上的碎木 D.ship、分段齊射 EVENTS e.sh、旗號 SIGNAL。現在收在 58_traffic.js shipDrop。只換 s.i 的話，被打過的堡壘一收掉，60_render.js mainOpaque 每一格丟例外。
- 🩸 **敵對的那一座等砲彈落地時要停手**：不停的話，它一直開砲、一直有砲彈在飛，永遠收不掉，還會打旁邊剛換上來的那一座。
- **怎麼測**：Blocksail 地球儀 → 航海、App 內瀏覽器、`__game.live(0.05)` 推；`__game.townFort('<地塊>')` 換上來；改 `G.ships()[0].px` 搬我艦。`go` 可能掉，要一直 post 到 `stats().t` 會走。
