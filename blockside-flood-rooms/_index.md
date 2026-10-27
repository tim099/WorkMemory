# 工作記憶索引 — blockside-flood-rooms

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_room-tier-heel-knobs** — 第 4 檔橫傾旋鈕 wvisc／wputr／wring／wroll 與量法
- **decision_sink-line-top-deck** — 開始沉的線照頂層甲板：跟著它要一起動的四樣

## knowhow
- **knowhow_local-vs-rebake-ruler** — 驗局部更新別用整艘重切當尺：改數洞

## pitfall
- **pitfall_measure-build** — 量測與建置的坑（無頭計時、worktree 建置、舊模型的步長）
- **pitfall_merge-dev-dt-reset** — 合併 dev：開局重設步長、讀數跟著 dev 走、?demo 對拍被分頁影響、Blocksail 要重套 battle.html
- **pitfall_render-sim-night1** — 房間水面與房間模擬第一階段的坑：return 換行、舊快取、漣漪上限、粒子落進灌滿的空腔
- **pitfall_room-split** — 切房間的三個坑（門、艙口、梁縫）
- **pitfall_room-tier-actn-zero** — 第 4 檔 fl.actN 永遠 0：照它判有沒有水的地方會靜默跳過（艙裡的水沒畫）

## pointer
- **pointer_leakcheck-static** — 漏水檢查（超載極限）是靜態自檢，不是模擬；砲門、門、舵在就不漏
