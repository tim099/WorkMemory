---
id: pitfall_cross-hz-hole-placement
topic: blockside-flood-rooms
title: 跨步長比進水量先對挖了幾格；wsim 讀數例外；半連通用 wpair 量
type: pitfall
status: active
created_at: 2026-10-28
created_by: calli
links: []
related_docs: [D:/Unity/Blocksail/external/Blockside/src/js/80_lab.js, D:/Unity/Blocksail/external/Blockside/src/js/81_floodlab.js]
---

跨步長（60／30／20／10 Hz）比進水量，先比 holes[].removed（2026-10-28，TASK-0810 第 3 格）：
- 進水報表打洞照 labSkin（80_lab.js）：那一站「離當下海面最近的船殼格」當水線，再往下 dz 格。步長不同 ⇒ 第 8 秒浮穩的姿態差幾公釐 ⇒ 有的站位進位到隔壁格 ⇒ 挖的格、破口面積就不同。
  血證：§2 那一組 10 Hz 四個洞高低差一格，挖 2,307 格、2.41 m²（60 Hz 2,266、2.16）⇒ 量出「10 Hz +13.8%」；四個洞的 dz 各調一格、挖同一批格之後是 −1.4%。
  ⇒ 比之前先對每一組的 holes[].removed 與第一筆 fl.area；不同就調那幾個洞的 dz（z 對齊）再量，⛔ 不要拿面積不同的兩組比模擬。
- 45b10d5（wroll）到 092c755 之間，報表 wsim=1、__game.wsim 一讀就丟例外（floodTilt 讀 fl.L.room，wrsRead 傳的小物件沒有 L）⇒ 整頁沒有 #lab、labrun 只說「沒讀到 #lab」。成品的 catch 只印 message ⇒ 查的時候把 index.html 那行 glFail(...) 換成 console.log(e.stack) 另存一份跑（labrun 加 --enable-logging=stderr 讀得到 console）。
- 半連通拉平要用 ?lab=wpair&semi=1 量（47e76df）；wop 控制實驗放的開口高度是「兩間地板較高那間＋1 格」，水面在它下面就什麼都量不到（三次都沒測成）。
- 房間內黏性 wvisc=50 會拖慢半連通與一般開口的「兩間拉平」（20 秒差 22 cm；wvisc=0 ⇒ 10 秒拉平）—— 取捨在 TASK-0809 等 Tim。
