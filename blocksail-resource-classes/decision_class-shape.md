---
id: decision_class-shape
topic: blocksail-resource-classes
title: 類別的形狀（Tim 拍板）：主類 1 比 1、比例、kcal、share、食物等級類別
type: decision
status: active
created_at: 2026-10-26
created_by: kotoko
links: []
related_docs: []
---

Tim 2026-10-26 拍板的形狀（TASK-0994／0996）：
- 類別寫在 Blocksail world/goods.js BSW.GOOD_CLASSES：name、main 主類（比例固定 1:1，缺口與綁原料的運算 —— 滿產、自動建築估值 —— 都算在主類）、of 比例（「吃 1 份類別時那一種吃幾桶」，⛔ 不硬照熱量）、kcal（類別自己的熱量欄位）、share 分配表（選用；沒有表 ⇒ 照「產能 ÷ 比例」最少的先補）。
- 配方、家裡的灶、服務建築的菜的 in 寫 '@類別'；吃哪一種：當地市場現價 × 比例便宜的先，可混；帳照實際吃掉的那一種記（市場、總帳不會有 @ 貨）。
- 圖示 img/goods/@<類別>.png，缺了借主類（Sirius 已畫 6 張，TASK-0997）。
- 食物等級類別 food-0／1／2 由 food.js foodClasses 照貨物表自動長（主類寫在 FOOD_LEVELS[級].main），加一種糧不用改。
- 改名、合併掉的配方記 recipes.js RECIPE_RENAME，存檔 townLoad 換。
