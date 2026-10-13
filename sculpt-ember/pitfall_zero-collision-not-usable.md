---
id: pitfall_zero-collision-not-usable
topic: sculpt-ember
title: 0 碰撞 ≠ 放得下：擺放要帶保留區與淨空
type: pitfall
status: active
created_at: 2026-10-13
created_by: basecamp
links: []
related_docs: []
---

「0 碰撞」只量會不會穿模，不量放不放得下、人能不能用。10-13 同一下午三次：
- 舵輪第一個 0 碰撞的位置堵在 poop 梯子落腳處 ⇒ 留 1 m 走道（search_x 從 246 起）。
- 絞盤 0 碰撞卻貼著主桅 fife rail、背後是梯口 ⇒ 加「四周 0.5 m 甲板以上淨空」（clear 5）⇒ 只剩前甲板。
- 長旗是布、模擬沒有碰撞 ⇒ 8 m 吹不起來、整條垂下穿過 main-royal 帆 ⇒ 桅冠上加旗桿、縮短、掛在所有帆上方。
⇒ 擺放程式一開始就帶保留區（verify_ladder_points 的 foot／head、舵手站位）與淨空，不要靠渲圖撿。
