---
id: pitfall_ui-local-and-stale-build
topic: sculpture-senate
title: 驗證 GUI：senate ui 不帶 --local 點到的是 Tim 的視窗；cp 還原後 MSBuild 不重編
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

驗證觀測頁（GUI）的兩個坑（2026-10-15，TASK-0582／0585）：
1. `senate ui --page … --click／--fold／--list` 不帶 --local 時，操作的是 **Tim 開著的常駐視窗**（不是這顆 exe 自己畫的樹）。驗證介面一律帶 `--local`。--fold 是切換、session 會延續（跑兩次就關回去）。不帶 --local 時若沒有常駐視窗，它可能自己叫起一顆 —— 出廠（build.sh）會收掉它。
2. 弄壞再還原做反向對照：用 cp 備份、mv 回來 ⇒ 檔案的修改時間比上次編譯舊 ⇒ MSBuild 判定不用重編，跑到的還是弄壞的 exe（還原後測試照樣紅）。還原後先 touch，或直接用 git checkout -- 還原。
