---
id: knowhow_showlod-mid-done
topic: blockside-render
title: 表演船中那一級做完了（TASK-1104）：接遠那一級、交接帶時看這裡
type: knowhow
status: active
created_at: 2026-11-01
created_by: erina
links: [blockside-render/knowhow_showlod-plan]
related_docs: [D:/Unity/Blocksail/external/Blockside/src/js/60_showlod.js, D:/Unity/Blocksail/external/Blockside/src/SHOW_SPEC.md]
---

**表演船中那一級做完了（TASK-1104；Blockside dev 59c69fc／24e9f5c）——遠那一級、交接帶接著做時看這裡**（erina 2026-11-01）

程式：js/60_showlod.js（SHOWLOD、showlodPrep 在 frameBegin 的 rbatPrep 前、showlodMid(o, sh) 給 rbatShip／rbatCheck／drawHulls／glsDraw 判）。烘：admin.py showlod ⇒ 成品 ?showlodbake=<船型>（⛔ ?showlod= 是開關，烘頁換了名字）⇒ ships/showlod.js。

接遠那一級：
- showlodPrep 已經分 mid／far（far 照中那一級畫，stat.far 分開記）⇒ 加 lv 2 的分支：rbatShip 整艘跳過、drawHulls 跳過、drawSails／繩／船員照 lv 跳過、陰影不畫、gpuSlots 不給遮罩。
- 貼圖可以照 showlodBakePage 同一頁拍（它已經開好一場、SHIPS[0] 是那一型），admin.py act_showlod 讀同一個 <pre id=showlod> 再多帶一個欄位。

會咬人：
- 船是空心的 ⇒ 照原樣 12/64 降採樣會破洞（信風號丟 21%）；要灌空氣、貼著船裡的塊都留（現在 1～7%）。
- 顏色要只讓從外面看得到的格投票，不然甲板、肋材的色搶贏、整艘偏淡粉。
- 量具：session 55fb05f7 scratchpad m1104/（1101 那一套抄過來：serve_ab.py 8823 多導 ships/showlod.js ⇐ SHOWLOD_JS 環境變數；drv.cjs DPORT 9573；lodmeas.js 同一格關／開數 draw call、三角形；pixnear.js 全部近那一級的開關逐像素比，港內有牆鐘雜訊要配對照組）。
- 遊戲包是一個閉包：頁面上沒有 window.render ⇒ 用 __game.showlod(v) 畫一格。
- drawArraysInstanced 的實例數是第 4 個參數（mode, first, count, instances），drawElementsInstanced 是第 5 個。
