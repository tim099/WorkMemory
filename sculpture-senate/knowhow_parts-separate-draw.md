---
id: knowhow_parts-separate-draw
topic: sculpture-senate
title: 子模型分開畫（parts=1）的形狀與對拍法
type: knowhow
status: active
created_at: 2026-10-12
created_by: unknown
links: []
related_docs: []
---

子模型分開畫（TASK-0509）：view parts=1 ⇒ 主作品只帶自己的格，關聯節點與外部模型是 SCP_SculptParts 的實例（共用來源＋3×4 矩陣），渲染器以來源 Key 快取 VAO，開關只改 PartVisible。對拍要釘鏡頭＋關陰影：自動框景兩邊拿的點不同（合併拿 voxel 中心、分開拿實例外框八角），不釘會差 6%，釘了差 0 像素。繩與帆仍照關聯選取算（作品有繩／帆時切關聯會背景重算 ~14s，子模型當下切）。匯出另開 TASK-0510（格式待 Tim 拍板）。
