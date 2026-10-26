---
id: pitfall_fort-flag-and-es-fort
topic: blocksail-fort-combat
title: 堡壘的旗照城的勢力；es-fort 還不能換成戰鬥單位
type: pitfall
status: active
created_at: 2026-10-26
created_by: basecamp
links: []
related_docs: []
---

堡壘的旗（TASK-0977，2026-10-26）：城裡建築模型的布旗沒有指定圖案 ⇒ 照規劃的 live.flag 掛國旗（Blocksail globe.html townLive 照範圍層的勢力給，Blockside 63_towns.js townNation）。
點堡壘換成戰鬥單位時，townFortWake 用 shipNation 掛同一面；作品那面白旗本來沒有貼圖，77_colours.js flagOwnUv 照 n × n 的布點補了 uv。換上來的那一棟，城裡的布旗不畫（以前兩面疊在一起）。

還沒做：es-fort（西班牙稜堡，basecamp-es-01m-fort，TASK-0975）點了不會換成戰鬥單位。Blockside 船型表的 town 只有 carib-fort、en-fort；要的話照 fort_carib 匯一座、town 填 es-fort。
