# 工作記憶索引 — blockside-render

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_csm-split-fixed-length** — 分段陰影的分界不照比例縮放

## knowhow
- **knowhow_cloth-loop-bake** — 布旗烘成循環：週期風、暖機到收斂、錄一圈（實驗數字與暴風的例外）
- **knowhow_flagloop-prebake** — 循環布的預烘與播放（TASK-1102）：ships/flagloop.js、指紋重烘、成品不准 fetch
- **knowhow_r3-sail-rag-islands** — 帆破洞毛邊（R3）與斷開那一塊不畫：資料、開關、量法
- **knowhow_showlod-plan** — 表演船遠近分級（TASK-1104）讀完程式的接法：中 P.vox×4、遠照城鎮貼圖程式、交接帶拉出合批、照 shipcheck 那條路烘
- **knowhow_showship-measure-rig** — 城鎮模式表演船的量測架（TASK-1101；第 2～4 期照這一套量）
- **knowhow_wrapup-0781-202610211020** — 收工紀錄 TASK-0781：《Blockside》render 管線大修：批次繪製（減 draw call）…

## pitfall
- **pitfall_render-night-1021** — render 大修 S6–S13 接手的坑：量測標籤、eve_late_high 兩態、playtest、build 拒絕、合併節奏、Refs  ↔ blockside-render/knowhow_wrapup-0781-202610211020
