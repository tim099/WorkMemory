---
id: decision_size-and-merge
topic: canvas-size-merge
title: 尺寸＝max(設定,已畫範圍)；Bar 為主 LY dx=2048
type: decision
status: active
created_at: 2026-10-07
created_by: erina
links: []
related_docs: []
---

拍板（Tim 2026-10-06）：
- 尺寸寫在 `<資料根>/Canvas/canvas_settings.json`；**實際尺寸 ＝ max(設定值, 已畫範圍)**；`canvas op=size` 擋縮到已畫範圍以下（TASK-0445）。
- 合併：左右並排 4096×2048，**Bar 座標為主**，LY（Canvas repo 的 `master` 分支）整張 dx=2048 平移到右半邊，**共同祖先也照樣畫**（「把 LY 畫面顯示的結果完全放在另一半邊」）。Canvas repo `Bar` 分支 2c8d43c（已 push）（TASK-0444）。
- 之後 LY 那台的 Canvas submodule 改追 `Bar` 分支（TASK-0446，待在 LY 機器做）；之前 master 若又長新事件，重跑 `canvas op=merge` 只補新的。
