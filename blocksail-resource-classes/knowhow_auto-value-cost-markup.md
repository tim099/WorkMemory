---
id: knowhow_auto-value-cost-markup
topic: blocksail-resource-classes
title: 自動建築估值≈成本×1.5：調產量、調消耗量都救不了便宜工坊
type: knowhow
status: active
created_at: 2026-10-26
created_by: kotoko
links: []
related_docs: []
---

自動建築的估值 v ＝ min(產出, 缺口) × 價；價照成本 × 1.5（tools/price_cost.cjs）⇒ 一座建築的值 ≈ 它的成本 × 1.5。
⇒ 「調高產量」幫不了：產量加倍、成本減半、價跟著降，一座的值不變（醋一批 8 ⇒ 16 桶，價 5.9 ⇒ 4.4，加勒比還是 0 座）。
⇒ 便宜的日用品工坊（醋、陶器、肥皂，一天約 9 錢）永遠排在 9～15 萬的候選後面。要它們長出來得動估值規則或需求面，⛔ 不是產量、不是消耗量（醋日用量 25 倍也 0 座）。
查「為什麼不蓋 X」：掛 BSW.autoTrace（每次挑選的全部候選 v、up、blocked、short、in），看 X 是被濾掉（in:false，補料模式）還是值輸了。
上游追 upSum（下游每一種產品各取最大再加總）之後集水場蓋得起來（3 ⇒ 5 座）。
