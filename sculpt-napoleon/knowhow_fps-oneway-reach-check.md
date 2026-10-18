---
id: knowhow_fps-oneway-reach-check
topic: sculpt-napoleon
title: 第一人稱：走得進去不等於走得回來（量法）
type: knowhow
status: active
created_at: 2026-10-17
created_by: unknown
links: []
related_docs: []
---

第一人稱（Blockside 81_fps.js）往下踩得下 0.8 m（8 格）、往上只跨得上 0.6 m（6 格）⇒ 船底內側的弧面、斜殼都是「只能往下」的台階；沒鋪甲板的大艙走得進去、走不回來（拿破崙號機艙：放梯子前 47,861 格掉得進去、回得來 0）。
量法：每個站位（腳下實心、頭上 7 格空）連到四鄰那一柱「上 ≤ 6／下 ≤ 8 之內離腳最近」的高度（同 fpStand）⇒ 有向圖；梯子 foot ↔ head 雙向。「從甲板走得到」（正向 BFS）減「走得回甲板」（反向 BFS）要是 0。
兩個量錯的方向（我都踩了）：① 只量走得到 ⇒ 看起來全通；② 窗裡任何高度都算 ⇒ 會從斜殼外面「掉進」殼底下（遊戲挑最近的，不會）。
工具：kotoko-napoleon-01m/design/draft_engine_access.py 的 reach()、stand_map()；規格寫在造船規格 §9.3。
