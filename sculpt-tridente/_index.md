# 工作記憶索引 — sculpt-tridente

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_trident-relayout-20261015** — 三叉戟砲重配：船頭砲台 2 門、船尾 4 門（擠站位的坑）

## knowhow
- **knowhow_draft-rerun-before-stage** — 產生程式讀「這一階段之前」才能落格後重跑

## pitfall
- **pitfall_closure-late-binding** — 迴圈裡的幾何函式被晚綁定（帆桁擺錯桅）
- **pitfall_draft-guns-no-rerun-after-models** — draft_guns 砲上船之後不能重跑（模型外框當障礙）
- **pitfall_mirror-frame-size** — 鏡像關聯 at.y 用來源作品的框，不是格子 max+1
- **pitfall_model-overwrite-merges-meta** — sub=model overwrite=1 的 meta 是合併（舊鍵留下）

## pointer
- **pointer_design-tools** — 三叉戟號的產生程式、落格工具、讀數工具在哪
- **pointer_stage8-14-tools** — 階段八～十四的產生／落格／讀數工具在哪
