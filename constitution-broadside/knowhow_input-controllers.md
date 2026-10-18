---
id: knowhow_input-controllers
topic: constitution-broadside
title: 控制器 71_ctrl.js 與輸入對拍腳本
type: knowhow
status: active
created_at: 2026-10-18
created_by: ame
links: []
related_docs: []
---

控制器（TASK-0668 ②，71_ctrl.js）：ctrlAdd 一份一種操作；ctrlDo 照 pri 由大到小問（回 true 接住）：fp 20 → build 10 → orbit 0；ctrlEach 每個都叫（down／mouse／look／frame）。每個控制器自己看開著沒（fpKey 關著也要收放開的鍵，不能先用 on() 篩）。加一種操作 ＝ ctrlAdd，不改 71_input.js。
驗法：模擬事件的輸入對拍腳本（scratchpad t679/inputtest.js：PointerEvent／WheelEvent／KeyboardEvent 打在 #cv、讀 __game.camera()／__game.fp()／#bldBar .bslot.on），改前改後同一支跑、逐項比。⚠ 藏起來的窗格畫面迴圈不跑 ⇒ 右鍵平移（CAM.pan → 下一格 camUpdate 才進 tgt）量不到。
