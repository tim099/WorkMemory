---
id: knowhow_town-fort-wake
topic: blocksail-fort-combat
title: 航海點城裡的堡壘：換成戰鬥單位、開火就敵對；怎麼在地球儀上測
type: knowhow
status: active
created_at: 2026-10-26
created_by: summit
links: [blocksail-fort-combat/decision_fort-is-fixed-ship]
related_docs: []
---

航海時點城裡的堡壘（Blockside dceb8a8，TASK-0955）：
- ships[].town ＝ 換的那一種城鎮模型。inPick 沒點到船 ⇒ townHitAt ⇒ townFortWake 載入、擺上、TOWN_UNITS[城:地塊] 記著；那一棟模型不畫、石基照畫。
- 中立 ＝ 玩家那一隊＋sh.calm；fireGun 瞄它或 ffHit 打中它 ⇒ fortProvoke 換到對面。Tim 原話「對堡壘開火就進入敵對狀態」。
- isFoe：npc 跟 fixed 互不理會。
- 擺法：城鎮模型與船的擺法手性差一個鏡射 ⇒ 照 x 鏡射對齊（townFortWake 上面有公式），換上來的前後顛倒。
- 測法：Blocksail 地球儀 __globe.place(lon,lat) ⇒ battleIn('sail')；iframe 重載加 &debug=1 之後鏡頭卡在進場過場（CAM.tr hold）⇒ __game.cam({tr:null})；藏著的分頁要截一張圖畫面迴圈才走、城才建出來；__game.townFort() 列、__game.townFort('<地塊>') 換。battle.html 換成自己核心：另寫 shell.json（core 指 worktree、out 指 scratchpad）＋只換 /battle.html 的小伺服器。
