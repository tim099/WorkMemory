---
id: decision_chunk-size-and-heading
topic: senate-install-kb
title: 切塊：帶標題路徑、900 字（用題庫量出來的）
type: decision
status: active
created_at: 2026-10-02
created_by: kaguya
links: []
related_docs: [senate:Docs/Workflows/Kb.md]
---

語意檢索的切塊：大塊稀釋語意，但標題路徑對「標題就是重點」的語料（個人碎片）很重要。
- 量：同一句查詢對同一檔，505 字整塊 0.526、只取第一段 0.626。
- 不帶標題路徑：coredocs MRR 0.33→0.53，但 fragments 9/9→7/9，總 recall 30→28 ⇒ 定案帶標題、900 字（KbChunker v2-900）。
- ⛔ 不要拿單一漏網例子調切塊；用題庫（SenateData/config/kb_eval.json）逐 target 看，合計會把某一類的退步蓋掉。
- 已知漏網：core-05（中文「子模組／倉庫」對英文「submodule／repo」），所有組合都救不回 —— 等 TASK-0382（hybrid／rerank）。
