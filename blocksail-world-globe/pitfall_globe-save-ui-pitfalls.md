---
id: pitfall_globe-save-ui-pitfalls
topic: blocksail-world-globe
title: 地球儀存讀檔與面板：同一物件比不出改動、details 收起還量得到高度、橫捲軸、進城按鍵太早
type: pitfall
status: active
created_at: 2026-10-24
created_by: unknown
links: []
related_docs: []
---

地球儀（globe.html）這幾輪踩到的：
- 🩸 開 Editor 版本時，手上那一份跟 Editor 版本是同一個物件 ⇒ edDiff「改了什麼」永遠是零。開頁就 saveCopy 一份。
- 🩸 收起來的 <details> 裡面的元素，新版 Chrome 用 getBoundingClientRect 還量得到高度（::details-content）⇒ 看不看得到一律用 checkVisibility()。
- 🩸 overflow-y: auto 會連帶開橫向捲動：面板出現直的捲軸變窄 2 px，「開戰」那個 select 突出 1.33 px ⇒ 多一條橫捲軸。overflow-x: hidden 只是藏起來、程式還捲得動；真正修法是讓含 select 的 label 可以縮（min-width: 0）。
- #top .row 的 display: flex 會蓋掉 [hidden] ⇒ 要另寫 [hidden]{display:none}。
- 量法：world_serve.py 帶 --saves、--start 換成暫存資料夾（網頁讀 world/start/ 也照 --start）⇒ 實測不動 repo；每一段開新的 Chrome 設定檔 ⇒ 接回來的只可能是檔案。
- 進城鎮模式之後馬上按 B 會被丟掉（海戰那一頁還沒 RUNNING）⇒ 量具「沒開就再按」；判斷進城看 #twbTown.on，不要用 __game（要 debug=1 才有）。
- 一行式 eval 字串裡塞 // 註解又吃掉後半一次 ⇒ eval 裡不寫註解。
