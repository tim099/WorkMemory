---
id: knowhow_showship-measure-rig
topic: blockside-render
title: 城鎮模式表演船的量測架（TASK-1101；第 2～4 期照這一套量）
type: knowhow
status: active
created_at: 2026-10-29
created_by: erina
links: []
related_docs: []
---

**城鎮模式表演船的量測架（TASK-1101；第 2～4 期照這一套量改前改後）**（erina 2026-10-29）

怎麼搭（腳本在 session 34fede82 scratchpad `m1101/`）：
- `serve_ab.py 8811 D:/Unity/Blocksail`：靜態伺服器，`/battle.html?…&v=名字` 換成同一個資料夾的 `battle_名字.html`。用 `admin.py shell <json>` 拿某一份 Blockside 成品包殼，json 的 core 指到那份成品。
- `drv.cjs`（DPORT=9561、POS=-2400,0）：常駐的有畫面 Chrome＋CDP，POST 一段 JS 在地球儀頁跑，`/shot?f=` 截圖。
- `fake.js`：攔 iframe 的 postMessage，把 'traffic' 清單整份換成固定 15 艘。巴斯特爾 11 個泊位＋港外 4 艘；泊位座標由經緯度換算：x ＝ 3129.12 ＋ (lon＋61.720758)·107008，y ＝ 1806.67 ＋ (lat−15.983656)·(−110541)。id 要每次一樣，⛔ 同一個 id 不會換位置重生。
- `setup.sh`（EXTRA='&v=名字&…'）：重開地球儀 → 裝假船 → 停港 → 城鎮模式（`window.__MODE='sail'` 換航海）→ 等 15 艘放好。
- `meas.js`：港內／港外兩個機位，清掉累計後讓遊戲自己的迴圈跑 15 秒。讀 `frames({sum:1})`、`prof()` 的 buf 與 survey.n、`chains(0).ms`、`simFar()` 的 crewN、`__rbat.stat`，最後 `rprof(5)`。`run.sh 名字 標籤 [網址參數]` 量一輪寫進 res.txt，`show.py res.txt 標籤…` 列表。
- `pix.js`：同一格 `__game.reduce()` 開、關各畫一次，逐像素比，出差值圖。`wake.js`：停住 → 接回。

讀數（改前 dev 1e96edc／改後 a7be99b，港內）：一格 29 → 9.7 ms；其中帆繩緩衝 16 → 0.8 ms；帆重做 15 秒 2,928 → 356 次。

會咬人：
- 同一格畫兩次本來就不同（樹、水面照牆鐘動）⇒ 逐像素比要先跑對照組。
- 小泊位相鄰 16 m，生船前的空位檢查會擋 ⇒ 相鄰的只放一艘。
- `__game.fire(ammo, aim)` 不設 aimOn：沒有敵艦時打不出去。要自己設 `me.aim ＝ [目標的格座標]`、`me.aimOn ＝ 目標船`；⛔ 把船物件塞進 aim ⇒ HUD 的 gunAims 報錯、遊戲迴圈停。
- 被打的往來船要約 60 秒才第一次齊射（轉身瞄準）；停沒停過都一樣，要盯滿 75 秒。
- 共用 Blockside 工作區常有別人沒提交的 ship_cards.json ⇒ 在那裡 `build --code` 會把它吃進成品、指紋變掉。合併後只檢查、不留那份重建（`git checkout -- index.html`）。
- ⛔ shell 裡 export 了 GIT_WORK_TREE（Blockside worktree）就不要跑 `senate cmd`：任務同步會拿錯的工作樹自動收一筆。
