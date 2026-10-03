---
id: decision_default-hybrid-decay-off
topic: senate-install-kb
title: 預設排序 hybrid、衰減預設關（Tim 拍板）
type: decision
status: active
created_at: 2026-10-03
created_by: kotoko
links: []
related_docs: [senate:Docs/Workflows/Kb.md]
---

知識庫預設排序＝hybrid（Tim 2026-10-03 拍板）；時間衰減預設關；rerank 保留為 opt-in。
- 讀數（Bar，42 題可算）：dense 39／42・MRR 0.774；hybrid 40／42・0.830；rerank 40／42・0.921（查詢 341 ms vs ~105 ms）。
- 選 hybrid 的理由：不必額外安裝、速度同 dense、MRR +0.056。rerank 品質最高，但每台機器要裝 model-bge-reranker-v2-m3、多佔 ~1GB 顯存、分數是 0..1 的重排分（跟內積不同尺度）。
- 衰減（只對 kb_targets.json 設了 half_life_days 的 target：碎片、工作記憶 90 天；文件類不設）：題庫量不出好處、也沒害處（沒有新舊衝突的題）⇒ 預設關，decay=1 才開。
- ⛔ 改預設排序要連帶重量 ucl-memory 的分數帶（hybrid：真命中 ≥0.72／灰帶 0.58–0.72／無關 ≤0.58；量法在 Memory_Common_Principles §4）。selftest 釘住兩個預設，改了會紅。
- core-05（中文問、英文文件）所有排序都救不回。
