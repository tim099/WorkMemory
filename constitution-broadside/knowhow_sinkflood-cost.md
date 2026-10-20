---
id: knowhow_sinkflood-cost
topic: constitution-broadside
title: 沉船進水給人看：花費跟水量走、同時最多 2 艘、斷船那一截拿掉
type: knowhow
status: active
created_at: 2026-10-20
created_by: unknown
links: []
related_docs: []
---

Blockside 沉船進水給人看（33_flood.js floodVis，TASK-0617）的花費：
- 跟灌進來的水量走、不跟步數走 ⇒ ⛔ 輪流算（每步算 N 艘、dt 累積補）省不到：八艘一起斷，每一步總花費一樣（227 ms 量過、退掉）。
- 現在：同時最多 FL_VIS_N＝2 艘，開始沉那一刻滿了就整場不算（flVisSlot）；?flvisn= 換上限。威力 1 六艘斷 8–21 ms → 3–6 ms；威力 10 八艘全斷 58→274 ms → 8→44 ms（遊戲內瀏覽器、floodVis 自己的計時）。
- 量多船沉：?debug=1&blue=…&red=…&start=1，__game.ignite(i, 1, 'mag') 一起炸前彈藥庫，__game.run(2) 一次跑 2 秒（再長 javascript_tool 會逾時）、讀 __game.prof().flVis×flVisN 的差。blast=10 保證斷船。
- 斷船那一截的水：flCut 照 sh.breakX／breakSide 整格拿掉；之後那一團的格被拿走記髒重算時照樣拿掉（flCellRedo／flHoleRedo 裡看 fl.cutS）。只在 floodVis 裡 ⇒ 船的動作一位元不變。
