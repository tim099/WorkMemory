---
id: decision_rope-end-yard-follows-declared-ends
topic: constitution-broadside
title: 繩端跟不跟帆桁轉：照作品寫明的 ends，座標端點才照網格猜
type: decision
status: active
created_at: 2026-10-25
created_by: basecamp
links: []
related_docs: []
---

繩端跟不跟帆桁轉（10_ship.js ropeYards → ropeEndYard），2026-10-25 起照作品寫明的 ends 判，不再只看網格距離：

- `model:<…>/<點>` 指到帆桁 ⇒ 跟那根；指到別的模型 ⇒ 不跟；找不到那個模型 ⇒ 照網格。
- 固定點（關聯路徑，例 `主桅/shrouds-S`、`桅-fore/sheave-aft`）⇒ 不跟 —— 那是綁在桅、甲板上。
- 座標、沒寫、`sail:`、`rope:` ⇒ 照舊 yardNear（0.3 m 內有帆桁網格）。
- ropeDecide（繩端綁定、斷繩）的帆桁那一格也走同一個判斷。

為什麼：shroud／stay／futtock 的上端寫明綁在桅的固定點，可是 hounds 離帆桁的 sling 不到 3 格 ⇒ 只看網格就被當成綁在帆桁上，轉帆時頂端跟著帆桁甩（12 艘、全部是寫明固定點的端點）。
量法：?shipcheck 開戰 2 秒帆桁轉 16°，舊規則（臨時 ?yold=1）58 個端點被帶走、最多 0.25 m；新規則 0 個。

Tim 2026-10-25：「繩端 目前想法是儘量都手動綁定 不在靠自動判定」⇒ 新作品的繩端寫明綁在哪（固定點或 model:），座標端點只是退路。
