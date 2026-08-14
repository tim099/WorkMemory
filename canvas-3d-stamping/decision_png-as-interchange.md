---
id: decision_png-as-interchange
topic: canvas-3d-stamping
title: 2D→3D 中介格式定為 RGBA PNG（alpha=mask）
type: decision
status: active
created_at: 2026-08-14
created_by: summit
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_Sculpture_3D.md, commit:4d6c971]
---

**Tim 2026-08-14 拍板：2D→3D 全面改道走 RGBA PNG**，不再直讀 events。

- 中介格式＝RGBA PNG，**alpha 就是 painted-mask**（透明＝未繪製＝不放 voxel）
- 成立條件（實算驗過）：RGB332 的 256 個 index 解出 **256 個相異 RGB** ⇒ index→RGB→index 往返無損
- 換來的價值：**人核准的那張圖，就是被貼進去的那份 bytes**（預覽與實貼不再是兩次 render）
- 三道閘門都在扣費之前：expect_pixels 不符(exit 4) / 越界(5) / 體積上限(1)
  - 越界**預設拒絕**不靜默裁切 —— 「只貼了一角」看起來完全像成功
- ⚠ expect_pixels 只餵引擎、**不當餘額閘門**：預授權面積由 Cmd 自己量（region 兩角算／PNG IHDR 讀）
  —— 用對方給的數字守自己的門，門就是假的
- 展品 region **由實際落地 voxel 反推**，多刀 union；union 對 `bbox`（無 margin）不對 `region`（含 margin），
  否則每刀往外爬一個 margin
