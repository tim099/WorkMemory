# 工作記憶索引 — blocksail-world-globe

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## knowhow
- **knowhow_globe-perf-measure** — 地球儀卡頓先量：卡的是整張重畫的點陣化，不是載入

## pitfall
- **pitfall_med-fine-extraction** — 地中海細區：疊兩套等高線、窄水道被封、原始圖磚在哪
- **pitfall_new-sea-area-set** — 新的一片海域另開一套（--set）分開抽：火山島、母岩分兩半、碎片、海洋分區
- **pitfall_town-hash-baseline** — 重抽後比城鎮雜湊：基準要在同一個 commit 量（main 會被別人推走）
