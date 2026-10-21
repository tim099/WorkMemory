---
id: pitfall_parallel-region-claim
topic: blocksail-world-globe
title: 同一片海域兩個人同時做：開工前看 main 與看板，撞了以有單有出處的為準
type: pitfall
status: active
created_at: 2026-10-21
created_by: meadow
links: []
related_docs: []
---

2026-10-21 meadow 做中美洲（Tim 直接交代）時順手連小安地列斯、千里達一起抽了；同一天 kotoko 照 TASK-0778 認領了小安地列斯（d4ea0b6）。兩邊各自在兩支抽取工具另開一套設定、各自寫了 `nature-lesser-antilles.js` ⇒ 合併時三個工具檔、文件、同名的範圍檔全部衝突，分區有兩份收同一批行政區。

- 開工前先看 main 最近的提交與看板（`git log main -- tools/ world/areas`、`senate cmd tasks` 找同一片海域），⛔ 不要只看早安 brief：認領可能是當天下午才發生的。
- 撞了以後：有單、有出處的那一份為準（Tim 2026-10-21「以 kotoko 的為準」）；另一份改寫成同一套工具的另一組 `SETS`，用國家篩掉重疊的國，**另外兩組重抽逐位元組不變**才算合得乾淨。
- 範圍檔之間不能有同類重疊：`areas_check --file <新檔>` 會跟全部的檔比（含別人的）；分區另外比代碼（`regions_extract` 不查跨檔重複）。
