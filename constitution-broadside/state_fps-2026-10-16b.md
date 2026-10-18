---
id: state_fps-2026-10-16b
topic: constitution-broadside
title: 第一人稱（TASK-0649）第四版：碰撞三段、跳得進洞、過梁低頭；量的時候會咬人的
type: state
status: active
created_at: 2026-10-16
created_by: basecamp
links: [constitution-broadside/state_fps-2026-10-16]
related_docs: [Blockside/src/js/81_fps.js, Blockside/src/README.md]
---

## 第一人稱（TASK-0649，81_fps.js）—— 2026-10-16 第四版合進 main 之後的狀態（basecamp）
- 現在（main bc2e01e；dev 還是 ame 推的 3bad5a0 ＝ 第三版）：玩家是另外一個角色 FP.c ＝ { pos }（不在 sh.crew）；開的時候借一個船員的座標（船長 ＞ 軍官 ＞ 任何站著的人）。
- 碰撞三段 FP.mode（設定裡「碰撞」那一排，data-fpm 0/1/2，fpMode）：
  - 0 正常：中心那一柱站得住、四個角只擋牆；走路不從艙口掉；在半空往下找 FP_FALL 16 m ⇒ 跳得進洞。
  - 1 穿牆：在牆裡高度不變；往上只跨 FP_NC_UP 0.3 m（⚠ 取捨：樓梯也照高度穿過去，換層用 E 或浮空 —— 照 0.6 m 會踩上憲法號舷牆邊的欄杆蓋板，兩個高度一樣分不開；等 Tim 說要不要換）；fpInside 四個方向碰不到船身 ⇒ 擋。
  - 2 浮空：空白鍵／C、面板 ⬆ ⬇ 上下；切回 ⇒ fpLand（往下找 ⇒ 開始浮空的地方 ⇒ 船員旁邊 ⇒ 才退出）。穿牆切回正常、腳下一路沒有地板也走 fpLand。
- 眼高 fpEye：身邊 ±2 格＋前面 0.2 秒會走到的柱，最低頭頂往下 2 格；FP.fz（腳）與 FP.eo（低頭）分開平滑，低頭 0.04 秒、抬頭 0.35 秒。
- 不擋模型（砲、桶）：Tim 說不用。
- 還開著：建造模式第一期 TASK-0648（由這一套衍生）。

## 量的時候會咬人的（新的在上面）
- 🩸 行中 `// 註解` 會吃掉後半行的程式，node --check 不報（語法照樣對）：第三版 81_fps.js 中了兩行（fpSet 沒建梯子清單、fpHide 沒 return），meadow 2b9db66 的 69_prefs.js:74 也中了一行（slowcam 不存檔，已交給她 tavern 25347）。掃描器：找「註解裡出現像程式的 ASCII（a = b; / return x; / f(...);）」—— 合 main 前跑一次。
- 無頭量第一人稱別用 `__game.live`（軟體渲染每格都畫，400 格要十幾分鐘）：成品快照裡把 `fpMap: function (r) {` 換成先加一個 `fpRaw: function () { return { FP, standZ, fpEye, fpStand, fpFrame }; }`，直接推 `fpFrame(1/60)`，整套 1.6 秒。⛔ 只改快照，不進來源。
- 按鍵：`window.dispatchEvent(new KeyboardEvent('keydown', { key: 'w' }))`（空白鍵要帶 code: 'Space'）。模式切換用點 `[data-fpm="n"]`，走的是真的路徑。
- 憲法號頭頂淨高（一格 0.1 m）：底艙 z42 1.7–1.8 m、梁 1.4–1.5；下甲板 z60 2.1、梁 1.7；砲甲板 z82 2.2、梁 1.7–1.9。挖洞測試：砲甲板 (279, 116, 81) 半徑 6。
- 新 worktree 是 CRLF、轉 LF 之後 git status 全部是 M（只是換行碼）⇒ `git merge` 會擋；用 `git -c core.autocrlf=false merge/status/add`。砍 worktree 要 --force（先確認 `-c core.autocrlf=false status` 只剩 staged）。
- _cache 空的：`python -I admin.py cache` 從雕刻作品重新匯出、跟指紋比（這次 12 艘都對上；跑超過 2 分鐘，放背景）。
- 預覽窗格藏著 ⇒ rAF 不跑；內建瀏覽器開不了 Temp 底下的檔 ⇒ 手機版面用無頭 Chrome＋Emulation.setDeviceMetricsOverride 截圖。
- 對拍別在途中重建同一個 index.html：先 cp 成快照，對拍讀快照。
