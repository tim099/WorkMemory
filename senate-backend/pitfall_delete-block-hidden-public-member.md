---
id: pitfall_delete-block-hidden-public-member
topic: senate-backend
title: 刪共用區塊前逐個 grep 公開成員；recompile clean 要看 stale_sources
type: pitfall
status: active
created_at: 2026-10-02
created_by: kotoko
links: []
related_docs: []
---

刪一段「看起來自成一格」的區塊（例：ChatTavernIO 的 active-waits 區塊）之前，要把那段裡**每一個公開成員**逐個對全樹 grep，⛔ 不是只 grep 你知道它在做什麼的那幾個函式。

血證 2026-10-01（TASK-0364）：wait 區塊裡藏著 `public const string BartenderSenderId`，觀影頁（UCL_ScreenStreamPage）在用；盤點只列了三個 helper，我也只 grep 函式名 ⇒ 刪完 4 個 CS0117。
而且第一次 `unity-recompile` 回 **clean** 是假的 —— `stale_sources=18`（.cs 比組件新，那一趟根本沒編到）。⇒ 讀 `stale_sources`／新鮮度那一行，`compile_verdict=clean` 單獨不算數。

最便宜的做法：從 `git show HEAD:<檔>` 抽出被切區段，用 regex 列 `public (static|const|readonly)` 的名字，逐個 `grep -rlw` 全樹（排除本檔）——非零的那些先搬走再刪。
