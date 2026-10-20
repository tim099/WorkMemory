---
id: knowhow_blow-style-fall
topic: constitution-broadside
title: 被打掉的演出 blow=fly｜fall、帆桁停在船上要記撐著的那一格
type: knowhow
status: active
created_at: 2026-10-20
created_by: unknown
links: []
related_docs: []
---

Blockside 被打掉的演出（21_gunnery.js blowModel／stepFly，TASK-0751）：
- blowStyle：模型 meta blow=fly｜fall ⇒ config/meta_kinds.json 那個 kind 的 blow ⇒ fly。帆桁（yard）規格表標 fall。admin.py metaspec 驗 blow（fly／fall）。
- 亂數：fall 照 fly 那一套的次數與順序抽（戰局的 RNG_B）⇒ 戰局逐字不變；FLY 只給人看，落水的水花是畫面的亂數（fxSplash 用 gaussX、無頭不訂閱）。
- fall 停在船上：中心掉過 1.5 m 之後碰到 sh.grid 的格 ⇒ f.rest、記 f.sup（撐著的那一格）；停著的每一步看 f.sup 還在不在，沒了就從現在的矩陣重來（M0＝M、c0＝c、R＝I、v＝船速）繼續掉。⛔ 第一版沒記 sup ⇒ 桅樓被打掉帆桁浮在半空（Tim 截圖）。
- 除錯：__game.blow(i, 名字, 砲彈速度)、__game.flyState()、__game.helm(i)（舵壞了的操舵倍率）。
- 71_input.js 同一個函式裡變數很多（hl ＝ 治療時間）：加滑桿變數前 grep 同一函式有沒有同名 var，⛔ 撞名不報錯、會把別格的 oninput 換掉。
