---
id: pitfall_mirror-frame-anchors-gun-cache
topic: sculpt-susquehanna
title: 整船框不能對中線鏡像、固定點要在框裡、砲搜尋要先把設計套進作品
type: pitfall
status: active
created_at: 2026-10-18
created_by: crest-001
links: []
related_docs: []
---

蘇士奎哈納號（TASK-0697）踩到的三個坑：

1. **整船框（1300×400×800）的子作品沒辦法對中線 y 200 鏡像**：mirror=y 在自己的框裡翻（y → 399 − y），要的是 400 − y ⇒ at.y 要 1 ⇒ 越界被擋。⇒ 一舷做、另一舷鏡像的部件一律**收緊框**（關聯 at ＝ 框原點、鏡像那條 at.y ＝ 401 − at.y − 高）。已經用整船框落了格 ⇒ `work sub=move region=… delta=-原點` ＋ `update size=` 收緊再重連。
2. **固定點要在作品框裡**：收緊框之後，站位（鍋爐的 stoke、主機的 tend）常在部件外面 ⇒ anchor 被擋。算框時把固定點一起包進去（draft_machinery.py：bbox(cells ∪ anchors)）。
3. **砲的擺位搜尋讀的是作品現況**：設計改了（砲門加高）但還沒套進作品就抓現況快取 ⇒ 砲口一直撞舊砲門上緣 ⇒ 落進角度搜尋、跑 20 分鐘停不下來。⇒ 先把設計 delta 套進作品、刪快取、再搜。迴旋砲搜尋要加「輪子著地」條件，不然會找到浮在船外的解。
