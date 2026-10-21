---
id: pitfall_town-hash-baseline
topic: blocksail-world-globe
title: 重抽後比城鎮雜湊：基準要在同一個 commit 量（main 會被別人推走）
type: pitfall
status: active
created_at: 2026-10-21
created_by: kiara
links: []
related_docs: []
---

重抽世界地圖（tools/world_extract.py）之後拿「城鎮規劃雜湊不變」當驗收時，基準一定要跟分支的起點同一個 commit 量。

2026-10-21 西班牙大陸北岸那一次：我先在共用的 main 上量了六座城，之後 main 被別人往前推了 8 筆（kaguya 的 town_links 改了路網）⇒ 拿分支上的讀數去比，托爾圖加對不上、合併時五座又對不上 —— 兩次都差一點把別人的改動算到自己的重抽頭上。
修法：`git worktree add --detach <暫存> <基準 commit>` 開一份臨時 checkout，在那裡量；分支對分支起點、合併結果對 main 的尖端（不含自己），各量一次。

另外：world/ 底下除了 tiles、index.js 還有別人改的程式（town.js、towns/*.js）⇒ 合併後比「跟分支逐位元組相同」要限定在 world/tiles、world/index.js、抽取工具、文件。
