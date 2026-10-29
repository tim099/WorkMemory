# 工作記憶索引 — blocksail-production

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_build-grade-by-density-pending** — 待拍板：高密度住宅要不要只能用營造 2、3 級蓋
- **decision_crop-field-render** — 田裡的作物：照塊分組、不投影子、3D 300 m、放大 1.8（菜 2.25）
- **decision_fields-block-roads** — 路穿不過田、田不擋自己那幾欄、田外框滿 4 m
- **decision_school-hospital-levels-1064** — 學校、醫院、捐獻、教育與健康水平怎麼定的（TASK-1064）

## knowhow
- **knowhow_add-a-crop-good** — 加一種作物貨（薑）：land.species、奢侈品權重、會被碰紅的兩支檢查、田的植株
- **knowhow_annex-grid-auto** — 附屬的田：4 m 格子、長方形自動蓋、量格子的邊
- **knowhow_crops-in-game** — 作物與田在遊戲裡：annex ⇒ 清樹、鋪土、種；貼圖集分張
- **knowhow_kitchen-meals-ingredients-1013** — 城鎮廚房28道菜與實際食材分列的資料邊界

## pitfall
- **pitfall_jobless-vs-market-price** — 失業多、田沒人：市場價卡住（開場糧食灌太多、開場需求 0）
- **pitfall_new-consumer-auto-build** — 加新的「有人吃」之前：上游值不看量、無上限的值往上游亂傳、缺貨訊號只記一道、整串建築順序會變（TASK-0976）
- **pitfall_reverse-control-new-tables** — 新表的反向對照要刪鍵；main 的 b.cost 被註解吃掉
- **pitfall_townbuild-shared-by-gen-farms** — townBuild 開場農莊也在走：改它要比六座城規劃雜湊

## pointer
- **pointer_consume-needs-prosp-1108** — 需要表、繁榮度（TASK-1108）的開關、讀數與開著的
