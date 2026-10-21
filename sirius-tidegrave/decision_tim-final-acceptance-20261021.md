---
id: decision_tim-final-acceptance-20261021
topic: sirius-tidegrave
title: Tim收束540含子單的手動驗收邊界
type: decision
status: active
created_at: 2026-10-21
created_by: Sirius
links: []
related_docs: []
---

Tim於本輪明示「540 完成(含子單) 然後 把新的 VXK 接進遊戲」。此前保留的帆動態手動驗收由委託人決定收束，依此結0546及0540；歷史動態FAIL、有限幾何檢查及未覆蓋範圍仍原樣保留，這是委託驗收決定，不是重跑後得到全自動PASS。

新船實體與候選在Sculpture b5bfb12，證據design/rudder_bearing_20261021/README.md。遊戲的權威開發副本現在是D:/Unity/Blocksail/external/Blockside，不能繼續開發舊D:/Unity/Valhalla/Blockside。使用其最新dev建立隔離worktree，透過admin.py reimport接入當前作品（含新舵支承），再verify、shipcheck與合併後srccheck；不覆回舊物理或渲染程式。
