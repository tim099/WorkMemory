---
id: state_state-2026-08-14
topic: canvas-3d-stamping
title: 收工快照：三 op 已 commit，AXIS_MAP 與署名兩格待拍
type: state
status: active
created_at: 2026-08-14
created_by: summit
links: []
related_docs: []
---

**2026-08-14 收工快照（summit）**

## 已落地並 commit
- `stamp2d`（畫布區域→3D）／`stampimg`（任意 PNG）／`slice`（3D→2D，stamp 的逆運算）
- `Cmd_Sculpture` 三個 op 接線完成（三段式計費：預授權→引擎→按實結算）
- 展品自動登錄（`--exhibit-id`，region 由落地 voxel 反推、多刀 union、bbox 欄）
- `export --out-dir`；ViewerPage 折疊分區 ＋ 切片區 ＋ 匯出路徑可設＋開資料夾
- SHA：`4d6c971`（主案）／`fcc1a74`（切片區＋匯出）／`34d0f5e`（ExplorerUtil）
- 資料：`3a0f641` 貼圖驗收 ／ `summit-lighthouse` 展品

## 待處理（拍板／施工）
1. **AXIS_MAP `y±` 上下顛倒** —— 修法一行（flip_v→True），但要拍板語意（新舊貼圖朝向不一致）。
   引擎歸 gura。測試已備：`letters/summit/tools/test_facing_upright.py`（exit=1 抓到）。
2. **多人展品署名走事件流** —— 未實作。設計見 pitfall_authorship-from-eventstream。
3. `_last_view.png` 之外，`exports/` 仍未 gitignore（匯出成品要不要入版控＝引擎方與 Tim 的決定）。
4. 3D 那座燈塔目前**是躺著的**（因 ① 未修）—— 刻意不掩飾。

## 已知不可信的東西
- 我早上的往返測試（112/112）**不能當正確性證據**，只能當一致性證據。
