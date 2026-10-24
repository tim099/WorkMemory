---
id: knowhow_road-stairs-wall-joins
topic: blocksail-town-roads
title: 路接地塊的樓梯、牆接建築：規則、坑、怎麼驗
type: knowhow
status: active
created_at: 2026-10-24
created_by: unknown
links: []
related_docs: []
---

路接地塊的樓梯（world/town.js roadStairs；TASK-0861 2026-10-24）：
- 只看路端（沒接別的路）、沿路方向射出去 ≤ 2 m 碰到的地塊外框；高差 ≥ 0.3 m 才擺；擺在最後一段、樓梯底那一點插一個頂點；放不下 ⇒ issue 'stairs'（town_report ② 「⚠ 要手動調整」，不算自檢不過）。
- 往下的樓梯：⛔ 別整一塊平地到低的那一頭（靠路那頭變成坑、坑邊的坡把路跟樓梯隔開）⇒ 把那一段路面拉成樓梯的斜線；Blockside 往下的台階「每一階頂齊它靠路那一邊」，斜線才在踏面底下。
- 路從側面撞上石基的牆（路沒對著地塊）⇒ 直的樓梯擺不上去，只報、不擺；沒石基的不報（中間是邊坡）。
- 牆接建築（wallJoins）：joins 存「進、出兩點」不存沿牆距離 —— Blockside 的世界座標有 kx 縮放，沿牆距離兩邊量不一樣；Blockside 自己 proj 回去。圓塔用 SPECIAL 的 join: { r }。
- 驗：town_plan 九座雜湊（金斯敦、托爾圖加村鎮城不該動）、town_report --all 1 --mutate 1、town_check 跟 main diff（只差耗時）。
