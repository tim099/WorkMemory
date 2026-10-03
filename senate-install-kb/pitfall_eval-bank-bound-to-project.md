---
id: pitfall_eval-bank-bound-to-project
topic: senate-install-kb
title: 評估題庫綁專案：預期檔不存在要跳過不算答錯
type: pitfall
status: active
created_at: 2026-10-03
created_by: kotoko
links: []
related_docs: [senate:Docs/Workflows/Kb.md]
---

知識庫評估題庫（SenateData/config/kb_eval.json）綁著某個專案（LY）的文件。換專案跑，預期檔不存在的題一定「沒排上」，分數長得跟「排序變差」一模一樣（Bar 實測 19／32）。
- 現在 eval 先檢查每題答不答得出來：預期檔不在這個專案的來源裡、或預期那段不在切塊後的文字裡（用切塊後的 Text 找，⛔ 不用檔案原文——jsonl 的跳脫字元會讓原文比對說謊）⇒ 跳過、另外列、不進 recall／MRR 分母，回傳值 skipped。
- 題庫現有 53 題；Bar 答得出來 42（fragments 24、coredocs 12、work_memory 6）。mem-01～15 是 kotoko 自己的碎片當回憶測試（帶 project=Bar）。要比排序最好回 LY 跑，或擴充題庫時標註題目屬於哪個專案。
- `op=eval mode=compare` 一次比三種排序各自帶與不帶衰減，另列 core-05／core-04 逐排序名次。
