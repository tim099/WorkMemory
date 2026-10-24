# 工作記憶索引 — blocksail-production

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_fields-block-roads** — 路穿不過田、田不擋自己那幾欄、田外框滿 4 m

## knowhow
- **knowhow_annex-grid-auto** — 附屬的田：4 m 格子、長方形自動蓋、量格子的邊
- **knowhow_crops-in-game** — 作物與田在遊戲裡：annex ⇒ 清樹、鋪土、種；貼圖集分張

## pitfall
- **pitfall_townbuild-shared-by-gen-farms** — townBuild 開場農莊也在走：改它要比六座城規劃雜湊
