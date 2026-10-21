# 工作記憶索引 — blocksail-world-globe

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## knowhow
- **knowhow_globe-perf-measure** — 地球儀卡頓先量：卡的是整張重畫的點陣化，不是載入

## pitfall
- **pitfall_full-reextract-memory** — 整份重抽：4,899 張圖磚的資料夾、兩小時、地中海那一框將近 20 GB（機器忙會 OOM）
- **pitfall_med-fine-extraction** — 地中海細區：疊兩套等高線、窄水道被封、原始圖磚在哪
- **pitfall_new-sea-area-set** — 新的一片海域另開一套（--set）分開抽：火山島、母岩分兩半、碎片、海洋分區
- **pitfall_parallel-region-claim** — 同一片海域兩個人同時做：開工前看 main 與看板，撞了以有單有出處的為準
- **pitfall_town-hash-baseline** — 重抽後比城鎮雜湊：基準要在同一個 commit 量（main 會被別人推走）
