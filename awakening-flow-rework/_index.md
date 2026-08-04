# 工作記憶索引 — awakening-flow-rework

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_age-rename-and-no-setter** — wake_count → age（Tim 拍板）；且 age 不可寫入、只可推導

## pitfall
- **pitfall_alarm-fires-on-normal-silent-on-loss** — 每天噴的 wake_count 🔧 是比對對象錯；而舊碼方向是反的（正常日叫、真掉一次 wake 沉默）  ↔ workmem:awakening-flow-rework/decision_age-rename-and-no-setter
- **pitfall_predicate-on-effect-not-cause** — 判準訂在結果上就會被繞過（同型三連）
- **pitfall_presence-snapshot-dead-import** — presence_snapshot 死 import：功能三天沒跑過，每次都印警告沒人看
- **pitfall_stale-premise-old-log** — 拿舊 log 當證據前先問前提還在不在（08-03 誤報 moved=15 實錄）

## state
- **state_2026-08-03-calli-cursor-and-limit** — cursor 兩階段提交 + limit 別名上線；未知鍵擋不了（schema 無 optional）；消費側三筆 QA
- **state_2026-08-05-morning-compare-fixed-p1-stashed** — morning 比對已修（6a3bb97）；P1 全套躺 stash 等 persona 遷移  ↔ awakening-flow-rework/state_2026-08-03-pushed-and-partly-superseded
- **state_2026-07-31-goodnight-shipped** — 晚安瘦身已 ship，四項待明早驗 ~~[superseded]~~  ↔ awakening-flow-rework/state_2026-08-03-pushed-and-partly-superseded
- **state_2026-08-03-pushed-and-partly-superseded** — 已 push；往返連號驗過，三項仍 pending ~~[superseded]~~  ↔ awakening-flow-rework/state_2026-07-31-goodnight-shipped, awakening-flow-rework/state_2026-08-05-morning-compare-fixed-p1-stashed

## pointer
- **pointer_where-things-are** — 規則 / 設計 / code / skill 各在哪  ↔ persona-identity-layers/decision_identity-layer-table
