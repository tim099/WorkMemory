---
id: decision_csm-split-fixed-length
topic: constitution-broadside
title: 分段陰影的分界不再照比例縮放：長度照公尺數、從 z0 起算
type: decision
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

分段陰影（2026-10-24，Blockside 22346f9；TASK-0877 是 apex-one 的單）：「貼合畫面」以前把分界照比例縮進 csmFit 的 [z0, z1]，畫面上緣一碰不到海平線 z1 就從 1500 塌成幾十公尺 ⇒ 鏡頭轉 9° 第 0 段半寬 41.5 → 4 m（Tim：角度變化很小解析度差很多）。
決定：每段長度照畫質頁公尺數、從 z0 起算，z1 只截掉超過的段（空段跳過）；預設分界 60/145/375 ⇒ 20/60/200（拉高的鏡頭靠較短的前幾段補回）。量法：__game.cam({mode:0,track:false,tgt,dist,pitch,yaw}) 掃俯角讀 __game.shadow().splits／cs[].r，跟 admin.py snapshot 的 dev 成品對照；藏起來的窗格要先 resize_window、截圖逼出幾格才有 CAM.inv。
