---
id: pitfall_rooms-hud-ropes-traps
topic: constitution-broadside
title: 艙房、HUD、浮空繩這一天的坑（同名函式、canvas 定位、shrink-to-fit、錨不是綁點）
type: pitfall
status: active
created_at: 2026-10-14
created_by: ame
links: []
related_docs: []
---

2026-10-14（wake #16，TASK-0568 與一串 Tim 的小需求）踩到的坑：

1. 同名函式靜默覆蓋：新寫的 `gunAxis(m)`（10_ship.js）跟 21_gunnery.js 的 `gunAxis(sh, g)` 撞名 —— 建置把 js 接成同一個閉包，後面那個蓋掉前面，開戰前的配置頁直接報錯、開戰後也會壞。
   ⇒ 新增全域函式前跑：`grep -n "^function \w*" js/*.js | sed 's/.*function \([A-Za-z0-9_]*\).*/\1/' | sort | uniq -d`（目前只有沙盒的 sbDraw 是合法重複）。
2. 全域 `canvas{position:absolute}`：編輯器裡的 canvas 會飛到頁面外（rect.top −1948）⇒ 自己的 canvas 一律寫 `position:static`。
3. `#botC` 用 `left:50% + translateX(-50%)` 定位：不寫 width ⇒ shrink-to-fit 只剩半個畫面寬，往裡塞東西（大圓鈕）主面板被擠成一條直的 ⇒ `width:max-content` ＋ max-width。
4. 選單裡最長那個選項（放不下的原因）會把 `<select>` 撐成整排，手機多出橫向捲動 ⇒ 限 max-width。
5. bash heredoc 寫中文給 python stdin 還是會壞（交付單、補丁）⇒ 一律 Write 成 .py 檔再跑。
6. 量具的坑：同時開 6 個無頭 Chrome，有一局讀數不同 —— 重跑就相同（船沒載完）⇒ ropecheck.py 預設 --jobs 3；對拍不一致先重跑一次再懷疑程式。
7. 正在跑報告／對拍時不要重建正式成品（它讀的就是那份）⇒ 新東西建到 scratchpad/mnew 再拷成 HtmlGames/ame_constitution_broadside/_xxx.html 測，用完刪。
8. 繩的「錨」是端點 0.5 m 內最近的實心格，常常抓到隔壁的結構（帆桁上的繩抓到桅）⇒ 判斷撐不撐得住要照它真正綁的東西（T.ropeYard、r.ma／mb、r.pa／pb）。浮空根源：綁的帆桁／模型被打飛或被斷開那一截帶走，繩沒跟著斷。
9. 掛著的模型（吊艇架上的小艇）正下方沒有甲板 ⇒ 舊的 foot 往下找到海底 z 0 ⇒ 開場就掉 ⇒ 改照吊索（hangFalls）。
