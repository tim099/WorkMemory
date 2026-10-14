---
id: pitfall_looks-right-2026-10-14
topic: constitution-broadside
title: 照字串找到註解裡的標籤、建置包進別人沒提交的設定、看起來在其實 alive=false 的燈、heredoc 跳脫變 NUL
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

## 10-14～15 晚上的坑（四隻都是「看起來對」）

1. **照字串找標籤，找到註解裡的那一個**：壓縮版第一版照 `html.find('<script type="application/x-vxk"')` 放網格池 —— voxelkit.js 的註解裡就有一模一樣的字串 ⇒ 池子插進程式中間、整頁 SyntaxError。只有 headless 的 #dump 變成 NO DUMP 才發現。⇒ 找真標籤要用「base64 內容夠長」之類的實質條件。
2. **建置包進了別人沒提交的設定**：Tim 在後台存了 game.json（22:41），我 22:45 為砲位圖重建 ⇒ 成品帶了新數值、提交只收 61_hud.js＋成品 ⇒ HEAD 的成品跟設定檔對不上。⇒ 建置前先 `git status` 看 config/ 有沒有別人的改動。
3. **燈「一直都在」其實 alive=false**：夜燈號的提燈掛在船首像手上、底下是海 ⇒ 模型的「腳」往下找到 z 0 ⇒ 第一次 survey 判掉落，模型照畫、點光源不收。用 `__game.lostModels(i)` 才看得到。修法：有固定點 `bail` ⇒ 腳是 bail 旁的實心格（10_ship.js）。
4. **heredoc 裡的跳脫**：python patch 寫在 bash heredoc 裡、字串有 `\\x00` ⇒ admin.py 多了 4 個真的 NUL（ast.parse 抓到）。跟 `\n` 同一族 ⇒ 有跳脫字元的一律用 Edit 工具或 Write 寫腳本檔。
另：站位找不滿最少人數的砲（三叉戟號船頭 6 磅砲被三叉戟砲擠掉）開局就是灰的 —— 40_crew.js 已改成往外 3 m 找空位補到最少人數；新船擠的地方記得看 `guns.filter(g => g.manned < g.crewMin)`。
