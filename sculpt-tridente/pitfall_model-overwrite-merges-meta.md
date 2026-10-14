---
id: pitfall_model-overwrite-merges-meta
topic: sculpt-tridente
title: sub=model overwrite=1 的 meta 是合併（舊鍵留下）
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

work sub=model overwrite=1 換模型時 meta 是合併不是取代：砲-S18／P18 從追擊砲換回 12 磅，舊的 arc=bow 留著（遊戲會當船頭追擊砲），模型／固定點／碰撞讀數全正常，只有 parts 的「meta 跟 verify 全等比對」抓到。換模型一律先 sub=meta meta=none 再寫完整的；驗收比 meta 全等。（lesson 庫也記了一條）
