---
id: pitfall_use-without-model-silent-skip
topic: blocksail-town-roads
title: 規劃的用途挑不到模型時遊戲靜靜跳過（只有 skip 看得到）
type: pitfall
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

規劃裡的用途在遊戲裡挑不到模型 ⇒ Blockside 靜靜跳過（`__game.town().skip` 才看得到），town_report／town_check 都不報。
2026-10-25：加拉塔、君士坦丁堡的海關（customs 只有 en、carib 兩套模型）就是這樣空了一塊。
量法：照 Blockside townPick 的順序（風格清單 → TOWN_USE_ALT → 特殊建築的任一款）逐塊查索引，scratchpad hb/nomodel.cjs 的做法；廣場（plaza）本來就是鋪面、不算。
新增用途或給城換風格之後先跑一次；遊戲裡拍城時順手讀 skip。
