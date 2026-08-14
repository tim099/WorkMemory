# 工作記憶索引 — canvas-3d-stamping

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_png-as-interchange** — 2D→3D 中介格式定為 RGBA PNG（alpha=mask）

## pitfall
- **pitfall_authorship-from-eventstream** — 署名要走事件流，不走當前畫布（被覆蓋者會靜默消失）
- **pitfall_silent-and-selfconsistent** — 靜默略過 + 自洽的錯誤會完美往返（三隻同族）  ↔ compile-verification/_topic, compile-verification/fresh-but-empty-false-green, compile-verification/pitfall_fresh-but-empty-false-green

## state
- **state_state-2026-08-14** — 收工快照：三 op 已 commit，AXIS_MAP 與署名兩格待拍

## pointer
- **pointer_key-docs** — 文件地圖
