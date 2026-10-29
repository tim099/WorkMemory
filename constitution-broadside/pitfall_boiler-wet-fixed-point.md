---
id: pitfall_boiler-wet-fixed-point
topic: constitution-broadside
title: 鍋爐泡水：動力件檢查預設關、固定點在實心格 ⇒ 永遠不熄；照砲浸水看爐口
type: pitfall
status: active
created_at: 2026-10-29
created_by: unknown
links: []
related_docs: []
---

動力件檢查（drive.partCheck）預設關著（Tim 2026-10-17）⇒ 31_drive.js drvPartsTick 根本不跑；而開著的那一條看鍋爐泡水用的是鍋爐的固定點 boiler-n —— 那一點在鍋爐的實心格裡，艙內的水格容量 0，wetAt inner 永遠 false。
⇒ 鍋爐從來不會淹熄（Tim 截圖：整艘泡水還有 80% 動力）。凡是「掛在 drvPartsTick 裡」的新效果，預設都不會生效 —— 第一版司爐的到崗倍率就是這樣白做（打倒司爐蒸汽照樣 1）。
修法（TASK-1092，3e1dac6）：照砲浸水（43_swim.js gunWet）—— 每座鍋爐的爐口 ＝ 旁邊站得住的地板往上 0.9 m（boilerFire，船型層級算一次），wetLatch 連兩次；倍率乘在 survey 裡（D.stk），動力件檢查開關都算。
量法：CDP 腳本 __game.hole(0, x, z, side, r) 在爐口旁兩舷開洞（side 是 1／0，⛔ -1 也是真值），讀 __game.drive(0).chain.stk、boilers[].wetOut。讀數欄位別撞名（第一版把 out 蓋掉了 boilers 原本的 out）。
