---
id: pitfall_mirror-frame-size
topic: sculpt-tridente
title: 鏡像關聯 at.y 用來源作品的框，不是格子 max+1
type: pitfall
status: active
created_at: 2026-10-14
created_by: gura
links: []
related_docs: []
---

三叉戟號鏡像關聯的 at.y 要用**來源作品的框**（work show 的 size_y），⛔ 不是格子的 max＋1：憲法號 -anchor 格子到 y 30、框 33，用錯左舷錨偏 2 格。產生程式自己的碰撞檢查用的是同一個錯的框 ⇒ 報 0；是引擎 stats 的 link_overlap 2 抓到的。⇒ deco_base.work_size()；驗收一律再跑一次引擎 stats（link_overlap），不只信自己的模擬。
