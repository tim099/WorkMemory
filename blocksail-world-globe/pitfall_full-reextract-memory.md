---
id: pitfall_full-reextract-memory
topic: blocksail-world-globe
title: 整份重抽：4,899 張圖磚的資料夾、兩小時、地中海那一框將近 20 GB（機器忙會 OOM）
type: pitfall
status: active
created_at: 2026-10-21
created_by: meadow
links: []
related_docs: []
---

整份重抽（`tools/world_extract.py`）從 2026-10-21 起要 4,899 張 z10 圖磚（加勒比海 2,452＋地中海 1,718＋中美洲與奧利諾科 729）＋博斯普魯斯的 tiles12；少一框 ⇒ 那一框安靜地退回 NE（sparse 的框一張都沒有才擋）。完整的那份在 meadow 的暫存區 `raw_all`（Senate 的記憶 blocksail-terrain-tiles 有路徑；暫存區會被清 ⇒ 不在了就要重新下載，先問 Tim）。

- 一次約 2 小時 15 分，地中海那一框尖峰將近 20 GB。2026-10-21 抽第二次（驗逐位元組相同）三次都在那一框 `ArrayMemoryError`（3.5 GiB 配不到）：同一台機器上別的 session 在跑。⇒ 挑機器空的時候、旁邊不跑任何重的東西（areas_check、areas_cover、nature_extract 都吃記憶體）。
- 對照組（舊工具＋新資料夾 ⇒ world/ 0 檔變）先跑：證明資料夾完整、抽取可重現；新框的讀數再跟對照組逐行比舊框那幾行。
- 新框不能跟舊框重疊（等高線會兩套），也要排在舊框後面（後面的框只換自己框裡的海岸線）。河口三角洲（烏拉巴灣、奧利諾科）要 `reefs=False`，不然泥灘被當成珊瑚礁。
