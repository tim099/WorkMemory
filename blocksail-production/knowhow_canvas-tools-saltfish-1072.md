---
id: knowhow_canvas-tools-saltfish-1072
topic: blocksail-production
title: 帆布進需要表、船上存糧次選菜記理論消耗、工坊零庫存換配方照 7 天估（TASK-1072）
type: knowhow
status: active
created_at: 2026-11-01
created_by: apex-one
links: [blocksail-production/pointer_consume-needs-prosp-1108]
related_docs: [Blocksail/docs/GOODS_BALANCE_GAMES.md:141, Blocksail/docs/PRODUCTION.md:1810, Blocksail/docs/TRADE_SIM.md:493]
---

TASK-1072（Blocksail main 975b6d2e / 6e1df3e7）修帆布、工具、鹹魚一年產量 0 的三個卡點：
1. **需要表取代基礎消費（CONSUME.base）後漏收的貨會掉成 0 常態需求**：`CONSUME.needs` 開著時 `consumeBase` 只留 `byValue`（礦、錠），帆布 `canvas` 沒進 `CONSUME.needs.list` 只剩 `bargain`（堆過 60 天才買）⇒ 沒人織就永遠 0 需求。修法：`CONSUME.needs.list` 補 `canvas`（`tier: 'better', money: 0.004`）。
2. **船上存糧 `provFill`（world/fleet.js）只對第一道菜記 `theo` 會餓死替代菜的原料需求**：`shipDishes()` 順序是硬餅乾配醃肉（i=0）→ 硬餅乾配鹹魚（i=1）。原本 `if (!i)` 只在 `i=0` 記 `theo` ⇒ 醃肉缺貨湊不滿時，船隊退而求其次想買鹹魚，卻不對鹹魚記 `theo`，鹽漬魚坊看不到缺口。修法：用 `theoG` 追蹤這趟已記過 `theo` 的貨，只要 `need > 0`（目標還沒補滿），往下試到的菜對 `!theoG[g]` 記 `theo`（硬餅乾、淡水、木柴不重複記）。
3. **工坊換配方 `recipeDay`（world/auto.js）對小日耗（d < 1 桶/天）、零庫存（s0 < refMin）的貨拿 30 天試算會被 `refMin` 墊底曲線擋死**：一座工坊一天產 1 桶，而小城或高單價貨（如工具 `price=59.9`、帆布 `price=21.9`）每天 `d` 只有 0.05～0.2 桶，市場曲線 `ref = max(d*30, refMin=10)` 被 `refMin=10` 墊底。若第一座工坊拿 `Lk=30` 天試算，過剩的 ~28 桶是 `refMin` 的近 3 倍，`marketPrice(..., steep)` 跌到基準價約 0.25 倍，第一座工坊永遠不敢換去補零庫存。
   - ⛔ 坑：不能只看 `d > 0` 就改用 `H=7` 天！生鐵（`pig-iron`）偶爾因 `bargain` 有微小 `d=0.2` 且庫存已 >10 桶，若無條件用 `H=7` 而熟鐵（`d=0`）用 `Lk=30`，煉鐵場會卡在煉生鐵不換鍛熟鐵。
   - 正確條件：`m.goods[g] && m.goods[g].d > 0 && s0 < BSW.marketRefMin(g) * 1000 ? H : Lk`（其中 `s0 = s + ex[g]` 含同輪前面工坊已排的 30 天過剩量）。第一座工坊在零庫存（`< refMin`）時照實際 7 天週期 `H` 試算，一旦排了一座（`ex[g] >= refMin`）或庫存已達 `refMin`，後續工坊照 30 天試算，不跟風扎堆。
