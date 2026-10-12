---
id: pitfall_measure-and-impl-traps
topic: constitution-broadside
title: 量具盲區與實作坑（rAF 暫停、sandbox 截圖、--list、IIFE 互蓋、無錨物件）
type: pitfall
status: active
created_at: 2026-10-12
created_by: ame
links: []
related_docs: [ArtGallery/HTML_GAME_WORKFLOW.md]
---

量具與實作上咬過人的地方（2026-10-12）：
- **預覽窗格在背景時 requestAnimationFrame 會停** ⇒ 模擬不跑、看起來像壞了。量測用 `?debug=1` 的 `window.__game`（start／run(秒)／fire／one／stats／cam／ships／balls／pieces）手動推進。
- **預覽窗格不載 sandbox iframe**；**無頭 Chrome 的虛擬時間推不動 sandbox iframe**（另一個行程，截到停在「載入中」）⇒ 加 `--disable-site-isolation-trials --disable-features=IsolateOrigins,site-per-process`，外框頁要跟畫廊同源（data: 外框嵌 127.0.0.1 會被私有網路保護擋）。WebGL 要 `--use-angle=swiftshader --enable-unsafe-swiftshader`。
- **`voxel_export.py --list` 只省格子那次匯出**，繩／帆／模型／外框照樣即時讀 ⇒ basecamp 還在造船時會對不起來，出貨前不帶 --list 整份重匯。
- **game.js 是一個 IIFE，函式名會互蓋**：船員的 kill 把碎屑的 kill(i) 蓋掉過 ⇒ 船員那組叫 killCrew／woundCrew。
- **錨點找不到格的東西**（ratline、三角帆、舵索）要改掛在旁邊的繩或模型上，不然被打斷時會留在半空。
- **只縮進水不縮抽水** ⇒ 調慢之後抽水永遠贏、永遠不沉（已改成一起縮放）。
- **齊射全瞄同一點**時讀數（14 中 14）正常、圖不對 —— 要看圖。
- shell 一次修改太長時 bash 解析會失敗（unexpected EOF）⇒ 修改腳本先寫成檔再跑。
