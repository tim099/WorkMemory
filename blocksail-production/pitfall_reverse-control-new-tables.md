---
id: pitfall_reverse-control-new-tables
topic: blocksail-production
title: 新表的反向對照要刪鍵；main 的 b.cost 被註解吃掉
type: pitfall
status: active
created_at: 2026-10-28
created_by: unknown
links: []
related_docs: []
---

加新貨、新建築、新的一本滿意度的反向對照：只關開關不夠 —— 新貨自動吃到基礎消費（CONSUME.base）、新菜單、新配方都會動狀態雜湊。
做法：探針的 SET 支援「-路徑」刪掉一格（obs1064/hash.cjs：分號隔開），把 GOODS／RECIPES／SERVE_RECIPES／BUILDINGS／SATISFY 的新鍵刪掉、改過的陣列設回舊值 ⇒ 兩年雜湊 ＝ main 才算數。
另：main 從 73b8f68（TASK-0983）起 world/auto.js 的 b.cost＝…、b.mat＝… 接在 // 註解後面被吃掉 ⇒ 自動蓋的建築沒有造價、營造廠的工錢 0；auto_check、jobs_check、recipes_check、build_check 的「沒有造價」紅燈都是它（2026-10-28 還沒修，等 Tim）。
