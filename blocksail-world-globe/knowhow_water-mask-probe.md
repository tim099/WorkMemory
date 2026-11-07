---
id: knowhow_water-mask-probe
topic: blocksail-world-globe
title: 遮罩與水體的量法：探針分類、湖整塊被跳過、三種水三條規則
type: knowhow
status: active
created_at: 2026-11-06
created_by: basecamp
links: []
related_docs: []
---

遮罩與水體怎麼量（TASK-1231，2026-11-06）：
- 探針：scratchpad sev/probe.js（session 8bb63351）—— 在 battle 頁用 __EV 重算 bedDryCalc，每 8 m 一格照最近的取樣點分「內陸水（水面 ≥/＜ −0.5）、海的水、低於 0 的陸地、陸地」，再看海畫不畫（遮罩＋河口場 S）。遊戲裡那一份遮罩跟探針逐格對過（差 0）。
- 塞維亞「一陣有水一陣沒有」量不出來：河在窗裡一路連到窗邊；反向對照要自己擺一排陸地格才看得到舊規則會遮。
- 真的 bug 在湖：整塊都在湖裡的塊 max0 ≤ 0 ⇒ bedDryTex 當成「沒有陸地＝海」跳過 ⇒ 河口場（船附近 4.6 km 框）外湖底下畫海（阿蘇埃 282,587 格）。38c27ec 修。
- 規則：海平面的水（底質是海、原始高度 < 0、沒有段 3）＝淹水起點；內陸水一律遮、只留接海的河口交接帶。⛔ 內陸水當成海的起點 ⇒ 恩里基約湖會蓋上 0 m 的海。?drysea=0／?dryinl=0 照以前。
