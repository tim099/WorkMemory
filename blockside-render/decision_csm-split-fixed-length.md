---
id: decision_csm-split-fixed-length
topic: blockside-render
title: 分段陰影的分界不照比例縮放
type: decision
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

分段陰影（TASK-0877 的後續；Blockside 22346f9，2026-10-24 meadow）：「貼合畫面」不再把分界照比例縮進 csmFit 的 [z0, z1]。
每段長度照畫質頁的公尺數、從 z0 起算，z1 只截掉超過的段（那段空的）；預設分界 20／60／200。
為什麼：照比例縮時畫面上緣一碰不到海平線，z1 從 1500 塌成幾十公尺，第 0 段一格之內細十倍（甲板上俯角 0.45 → 0.6：半寬 41.5 → 4.0 m）。
改後甲板上 19 → 14.7 m 連續；拉高 200 m 跟以前一樣、300 m 陡俯略粗（117 → 128）。量法：__game.cam 掃俯角 + __game.shadow() 的 splits／cs[].r，shadowCover 看斷開。
