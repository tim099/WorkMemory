---
id: pitfall_borrowed-check-scope
topic: sculpt-frigate
title: 借別人的檢查程式先看它量到哪：梯子頭頂淨空封頂在甲板
type: pitfall
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

借別人的放置／檢查程式（例：憲法號 design/draft_orlop_ladders.py 的 Orlop.try_one）之前，先讀它每一道檢查**量到哪裡為止**。
- 血證（TASK-0685，2026-10-17）：try_one 的頭頂檢查是 `range(zs+1, min(zs+1+headroom, zu))` —— 封頂在上層甲板高度 zu ⇒ 梯子上半段穿出艙口之後的頭頂不看。風暴角號 hold-aft 頂上只空 6 格（上面壓著 下↔上 的 aft 梯子），檢查全綠、Tim 第一人稱一爬就撞頭。
- 修法：在自己的子類別包一層不封頂的爬梯淨空 —— 每一階踏板往上 18 格（Blockside 81_fps.js FP_EYE 16 ＋ FP_EYE_GAP 2）不能有實心、自己那座不算（summit-stormkaap-01m/design/draft_hold_ladders.py `try_one`）。
- 拿同一把尺量過：餘燼號、憲法號的貨艙梯每一階往上最少 27 格 ⇒ 沒中（2026-10-18）。
