---
id: knowhow_per-type-cache-at-ship-creation
topic: constitution-broadside
title: 開船照船型快取：怎麼找重的、怎麼驗逐欄相同、怎麼在忙的機器上量
type: knowhow
status: active
created_at: 2026-10-23
created_by: basecamp
links: []
related_docs: []
---

開船（makeShipSteps）照船型快取時怎麼量、怎麼驗（2026-10-22 TASK-0834 蓋板與砲口、0835 繩的錨）。

- 先找誰重：在一份成品副本（probe.html，不提交、量完刪）裡用正規式把函式包一層計時／計數（`function X(` → 改名 `X__o` ＋ 包一層），makeShipSteps 的每一步也能包。8 艘同型、型別快取熱的時候量。
- 判準「能不能快取」：讀程式確認它只讀船型（T.base／T.hullG、r.pts、meta）跟開局那一刻的狀態；讀到 modelLocal（砲塔轉了）、sh.grid（被打過）的 ⇒ 只在開船那一刻用快取，戰鬥中途懶建那條路不動。
- 做法：同型第一艘照舊算，全部算完才掛到 T 上（分段開戰半途不會被別艘拿到半份）；之後每艘造自己的狀態、幾何複製（陣列 slice）。物件鍵順序照原本那一行。
- 驗：① 每艘開好的結構序列化（物件參照換成序號）新舊逐艘雜湊相同、並確認這一輪真的走了快取；② 反向對照：改壞快取一格 ⇒ 雜湊要變、改回 ⇒ 回到原值；③ 戰局對拍（seed、150 秒、每 0.5 秒狀態雜湊＋各彈種發數）同型對同型、不同型各一組。
- 量時間：機器負載會讓同一份成品前後差 3–4 倍（64 vs 180–287 ms）⇒ 以工作量（呼叫次數）為準，毫秒只看同一頁連續量的比例。窗格藏著時背景分頁的 setTimeout 被節流，150 秒的對拍一場要兩三分鐘。
- 量到的：開一艘的大頭是 portsFloodInit（蓋板 portsBuild ＋ 每門 portHole，約 45%）、繩的兩端 anchorNear（每艘 1,850 次）；房間、進水格、走路格 < 2%、船型層同型只算一次（沒有重複解析）。
- 本機假伺服器的控制端點要 no-store、每輪換網址 ⇒ 不然自己的 reset 被快取、整輪作廢。
