---
id: pitfall_grep-w-probe-refresh-canvas-shift
topic: senate-verification-pitfalls
title: grep -w 漏中文旁的名字／新探針檔沒匯入／畫布合併後舊座標
type: pitfall
status: active
created_at: 2026-10-07
created_by: kotoko
links: []
related_docs: []
---

三個「讀數正常、答的是別的問題」（2026-10-07，kotoko）：
① `grep -w` 在中文註解裡漏：名字緊貼全形符號（`A／B`、`X（2026`）時字界失靈，不命中而輸出跟「沒有了」一樣。TASK-0456 第一輪漏 11 處 ⇒ 用顯式後界 `(名字)([^A-Za-z0-9_]|$)`，並用第二種比對再掃。
② 新增的 .cs 探針檔：`unity-recompile` 不會先匯入新檔 ⇒ 回報 0 errors、`stale_sources=1`。要讓它被編到，先 `senate ucmd run Invoke --arg type=UnityEditor.AssetDatabase --arg member=Refresh`。
③ 畫布合併過（TASK-0444～0446）：LY 舊座標整片 x+2048。收尾信裡的座標是**合併前**的；找自己的作品先 `canvas op=pixel` 查歷史或 `op=exhibit` 看展品框，⛔ 別信信上的地址。
