---
id: knowhow_rate-history-import
topic: exchange-portfolio
title: 歷史匯率跨區匯入 rate op=import 的規則
type: knowhow
status: active
created_at: 2026-10-09
created_by: unknown
links: []
related_docs: []
---

歷史匯率跨區匯入 `rate op=import`（region=Florin 或 from_ref=origin/LY；預設試算、confirm=1 才寫；TASK-0475，SCP_Core cf808f9＋Senate 09b5d02）。
- 每幣只取「比本地該幣**第一個**點更早」且「比本地**最新一版**更早」的版本 ⇒ 兩邊都抓的幣（BTC／GOLD）不交錯；匯入版不冒充最新版（NeedsPreSyncArchive 拿最新版比）。
- origin=`import:<ref>@<sha>`；每日閘 HasSyncVersionOnOrAfter 只數 sync ⇒ 不受影響。同代號跳過（重跑 0 版）；來源本身是匯入版不轉手；本地歷史有壞檔整份拒絕。
- 2026-10-09 已實際匯入 LY 8 版（JPY 等 7 種從 10-01 起）。main 快取補了 TWD/JPY/EUR/GBP/CNY/KRW/HKD 的 fx_rates_per_usd 端點 ⇒ 隨 demurrage op=run 每日刷新、且可互換。
- ⚠ 10-09 11:59 那一版 pre_sync 的 7 種是 LY 10-08 的價（op=set 初值），走勢上 10-08→10-09 的 0% 是它。
- 頁面：RateAdminPage「從其他區匯入歷史匯率」—— 確認那一下重新試算，sha 或版本數變了就拒絕。
