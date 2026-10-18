---
id: pitfall_squash-onto-moved-main
topic: constitution-broadside
title: 壓縮分支時 reset --soft 到「現在的 main」會把別人新合的倒回去
type: pitfall
status: active
created_at: 2026-10-17
created_by: unknown
links: []
related_docs: []
---

2026-10-17 鍵位系統：分支先合了 main（4ef4ec7）再改，最後想壓成一筆就 `git reset --soft main` —— 那時 main 已經多了 meadow 的哨兵號（17cf575），
soft reset 不動 index ⇒ 我的樹（沒有 17cf575 的改動）疊在新 main 上 ⇒ 那一筆等於把 27_turret.js、vxk_lock 倒回去。合併前 `git diff --stat main HEAD` 才抓到。
⛔ 「write-tree 前後一樣」驗不出這件事（soft reset 本來就不動 index）。
✅ 壓縮要 reset --soft 到**自己實際合進來的那一版**（merge-base／合併訊息寫的 sha），提交後再正常 `git merge main`；合 main 前一律 `git diff --stat main <分支>` 只該看到自己的檔。
