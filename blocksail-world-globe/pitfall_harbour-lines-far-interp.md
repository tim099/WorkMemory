---
id: pitfall_harbour-lines-far-interp
topic: blocksail-world-globe
title: 港口改線會讓幾公里外的海底跟著變深
type: pitfall
status: active
created_at: 2026-11-03
created_by: unknown
links: []
related_docs: []
---

## 港口改線（harbour_deep）會讓幾公里外的海底跟著變深（TASK-1178 聖喬治量到）

- 改線只改範圍裡的 5／10／20 m 線本身，可是海底的深度是照「離每一層線多遠」內插的 ⇒ 20 m 線多伸進港一段，附近淺棚、礁湖裡的格子離它比離原本的 20 m 線近 ⇒ 內插跟著變深。
- 聖喬治（depth 12，延伸 5、10、20 m）：16 m 一格、r 7 km 改線前後逐格比 ⇒ 變深 291,199 格、變淺 418 格；1 km 內中位 1.7 m；2 km 外最多 2.2 m、中位約 1 m；最遠 7.8 km（北礁湖 13.1 ⇒ 14.2 m）；陸海翻 0 格。
  範圍外的窄水道也會被帶深：Town Cut 的口（不在範圍裡）0.5 ⇒ 5.9 m。
- 量法：改之前、改之後各跑一次 `tools/harbour_grid.cjs --town <id> --r 7000 --step 16`，逐格比 h；再用範圍多邊形分「範圍內／外」、照離範圍邊的距離分桶。⛔ 只看範圍裡的讀數會以為「只動該動的地方」。
- 要避開某條水道（Town Cut 1917 年才挖開）：`--half` 收窄到範圍不含那個口（150 ⇒ 80 m），口外照舊淺；被內插帶深的那一段照實寫。
- 套法同 bathy_base 那條：main 的 `bathy_extract.py --apply` 會順手漂 12 個別人的檔 ⇒ 還原，索引照 HEAD 只換自己那一檔的列、接上自己那一筆的 edits／mask，editsHash ＝ 表檔 sha1 前 12 碼（合 main 時照合併後的表重算）。
