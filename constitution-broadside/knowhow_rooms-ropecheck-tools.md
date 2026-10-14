---
id: knowhow_rooms-ropecheck-tools
topic: constitution-broadside
title: 艙房、掛著的模型、繩的檢查：新量具（__game 與 ropecheck.py）
type: knowhow
status: active
created_at: 2026-10-14
created_by: ame
links: []
related_docs: []
---

2026-10-14 新量具（《憲法號對轟》，都在 ?debug=1 的 __game 或 _build 底下）：

- 艙房：`__game.roomPlans(id, type)`（每一間的格子、門口走道、自動擺滿的結果、ms）、`roomSet(id, name, v)`、`roomPlace(id, name, type, items, p)`、`furnHit(i, name, k, dmg)`（打物品看 buff／能力）。
- 甲板的取用點：`hullDeckKit(T)` 用船型的模型做空殼跑 gunDecks／makeMags／makeStations ⇒ 配置頁不開戰也有預設取用點。
- 掛著的模型：`__game.hangs(i)`（吊索、擺角、擺長）、`lostModels(i)`（開場就掉的抓這個）。
- 砲在 HUD 放哪：`__game.gunDirs(i)`（追擊砲照砲軸偏船頭／船尾）。
- 繩：`__game.ropes(i)` 現在浮空的繩、`ropes(i, true)` 作品本身每一端靠什麼；`python -I ropecheck.py [--secs 150 --seeds 1,2,3 --match a,b … --jobs 3 --out x.md]` 跑很多局寫報告（修前 481 → 修後 0）。
- 測試艙房（雕刻還沒標時）：scratchpad 的 injectrooms.py 把 rooms.py scan 的候選塞進 .vxk、mroom.sh 建成 _roomstest.html。
- 提交照舊：HEAD 設定檔＋自己那幾格建在 scratchpad/mnew，`git hash-object -w --path=<路徑>` ＋ `update-index --cacheinfo` 只放自己的 blob（工作區設定檔裡 Tim 後台改的 lim 不收）。
- 設定後台：admin.py serve 多了 `/game`（成品），後台頁嵌 `?admin=rooms` 編甲板與艙房的預設，postMessage cb-ready／cb-defaults／cb-rooms。
