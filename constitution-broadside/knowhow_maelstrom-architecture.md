---
id: knowhow_maelstrom-architecture
topic: constitution-broadside
title: 大漩渦怎麼接進遊戲（VX 單一入口、參數表、量法）
type: knowhow
status: active
created_at: 2026-10-18
created_by: kiara
links: []
related_docs: []
---

大漩渦（TASK-0683，kiara 2026-10-17/18，dev b51a6b8）怎麼接進遊戲 —— 改它之前先讀這段：

- 只有一個入口：08_sea.js 的 `VX`（vxInit 照 WEATHERS.maelstrom.vortex 建；別的天氣 VX ＝ null ⇒ 每一處都走原本那一條，晴天／暴風雨對拍逐字不變）。新效果一律寫成 `if (VX)` 的分支，⛔ 不抽 rnd()。
- 地形進 `seaH`（物理）與 SEA_GLSL 的 vxH／vxG（畫面），兩邊同一個式子：−D·rc²/(r²+rc²) − De·re²/(r²+re²)。船靠 stepWaves 船底十點擬合自己沉進漏斗、傾斜。
- 水流 `vxFlow`（切向＋往中心吸＋眼的吸力）加進 sh.vel ⇒ sh.vel 是**對地**速度（砲彈帶的船速、瞄準前置、帆的視風都照它）。碎片、落水的人也吃 vxFlow。
- 吸力 vxDrift（照離中心）、下坡加速 vxSlopeAcc（當推力加進 31_drive.js 的 Fx）、繞行轉向 vxTurn／眼裡自旋 vxSpin（直接加在 yaw）、眼的束縛 vxHold（乘 sh.spd）、眼往下拖 vxDrag（sh.vxSink 加在吃水上）、眼吞沉船 gulp（32_hull.js）。
- 參數全在天氣設定頁（08_weather.js WXK_ITEMS → 64_cfgkit.js cfgPage → 69_prefs.js wxk 只存改過的）。新增一項 ＝ WXK_ITEMS 一列 ＋ vortex 預設一格 ＋ vxInit 讀進 VX。
- 量法：無頭 Chrome `?demo=120&dump=1&seed=3`（＋`ship=falcon,stormkaap`、`demo=150&seed=2&cas=1`、`weather=storm`）比 #dump 的 sha1，dev 的成品用 `git show <dev>:index.html` 取；大漩渦自己跑兩次要一樣。行為用 __game：prefs({wxk:{…}}) 改參數、ships()[i].px/pz 硬放位置、drive.vT 讀終端航速。
  ⚠ prefs 的 wxk 只套給的那幾項 ⇒ 前一次設成 0 的，下一次要明寫回預設，不然兩次測的是同一組（我踩過一次）。
