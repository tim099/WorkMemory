---
id: knowhow_headless-game-measure
topic: constitution-broadside
title: Blockside 無頭量測：?debug=1、run 與 live 的差別、閉包函式的 hook
type: knowhow
status: active
created_at: 2026-10-17
created_by: basecamp
links: []
related_docs: []
---

Blockside 無頭量測（scratchpad/cdp_err.cjs，Chrome headless＋swiftshader）：
· 一定要帶 ?debug=1 才有 window.__game（00_core.js DEBUG）。__game.ships() 回的是活的 SHIPS（sh.cl、sh.sails、sh.ropes、sh.hs… 都讀得到）。
· __game.run(秒) 只跑模擬、快；__game.live(秒) 每一格都畫，在 swiftshader 上幾秒的 live 要幾分鐘 ⇒ 量模擬用 run，要畫面緩衝（sb.n、lines、tDraw）只補 live(0.05)。
· 閉包裡的函式（furlBundle、tearCloth、blowModel、rptShow…）外面叫不到 ⇒ 成品快照 sed 'fpMap: function (r) {' 前面塞 hook；⛔ 不進來源。按鈕類的（📊 報表）直接 click。
· 船型要先 await __game.loadHull，多艘用 __game.fleet([...], 模式)；__game.cmd(我方, 敵方) 設指揮官策略、__game.report() 讀戰鬥報表。
