---
id: pitfall_food-rules-unmeasurable
topic: blocksail-resource-classes
title: 起始檔不缺糧：糧食平衡規則要用手做小城量；foodBal要效率門檻
type: pitfall
status: active
created_at: 2026-10-26
created_by: kotoko
links: []
related_docs: []
---

起始檔兩年都不缺糧（每座城飢荒 0）⇒ 糧食缺口的規則（foodBal 平衡產能、food 類別）在真實起始檔上量不到：開關兩年逐位元組一樣。要量就用手做的小城（tools/class_check.cjs ⑦⑧：200 人、一座玉米農莊、地塊長得了玉米和大麥）。
foodBal 不加門檻會把缺口給熱量產能 0 的蔬菜田（一座只有玉米的 1/3）⇒ AUTO.foodBalMin 0.75：只在一天熱量 ≥ 最好那種 75% 的糧之間平衡（tools/auto_check.cjs ① 抓到的）。
