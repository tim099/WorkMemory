---
id: knowhow_paddle-wheel-as-propeller
topic: sculpt-susquehanna
title: 明輪在遊戲裡當螺旋槳：kind=propeller、axis 0,1,0、兩舷各一件平移放、兩具主機兩段軸
type: knowhow
status: active
created_at: 2026-10-18
created_by: crest-001
links: []
related_docs: []
---

明輪（side-wheel）在遊戲裡就當螺旋槳：每具一件外部模型 `kind=propeller`、`axis 0,1,0`（往前走時輪頂往前 ＝ 右手定則繞 +y）、`blades`＝槳板數、`diameter_m`、可選 `rpm_max`（蘇士奎哈納號 12，BARTOL 試航約 12 rpm）。31_drive.js 有幾個 propeller 就平分動力（兩具各 50%），轉速照推力算。

- ⛔ 模型一定要**平移放**（at ＝ pivot ＝ 軸心格心、scale 1、不轉不鏡像）—— 不然匯不出格子（rig.models[].vox）⇒ 兩舷各一件子作品（左舷那件照 y ↔ 400 − y 鏡像格子，不能用 mirror）。
- `hub` ＝ 軸心那一格；`shaft-fwd` ＝ 軸碰到船殼那一格（遊戲把輪子掛在它旁邊 8 格內的船殼上）。
- 主機要推到兩具：兩具主機（每舷一具）＋兩段軸（kind=shaft，fwd 在主機曲柄、aft 在船殼牆裡、離 shaft-fwd 1 格）⇒ 主機接 fwd 最近的軸、軸接 aft 24 格內最近的輪 ⇒ 匯入檢查「額外動力：明輪-S 50%、明輪-P 50%；主機 2、軸 2」。
- 輪子在明輪罩裡轉：量「每格繞軸轉 0..360（每 2°）落點碰不碰罩子／船殼」⇒ draft_paddles.py 的 wheel_sweep_hits_max。
- 實例與程式：`crest-001-susquehanna-01m/design/draft_paddles.py`、`tools/apply_paddles.py`（TASK-0697）。
