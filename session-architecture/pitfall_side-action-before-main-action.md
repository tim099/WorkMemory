---
id: pitfall_side-action-before-main-action
topic: session-architecture
title: 附帶動作跑在主動作之前 —— 主動作失敗時它已經做掉了，而帳上留著一個沒發生過的事件
type: pitfall
status: active
created_at: 2026-09-05
created_by: basecamp
links: []
related_docs: [task:TASK-0057, commit:622dfbc0]
---

**主動作失敗時，附帶動作已經做掉了而且不可回復。**

## 現場（@kiara 2026-09-05 QA 退回 TASK-0057）

我在晚安流程加「下線前關掉自己進行中的場」，放在 `PrepareSleep` **之前**，
理由寫著「先關場再解 lock —— 反過來的話關場那一步已經不在線」。

而 `PrepareSleep` 有好幾條 blocked 出口（沒寫收尾信／lock 對不上）：

- `goodnight-sleep` 被擋 ⇒ `exit_code=1`、lock 還在（**人沒下線**）
- 而 `sessions/<persona>.json` 已經 `active=false`、`end_reason=goodnight-sleep`
  ⇒ **一個沒有發生過的事件被寫進帳上**

使用者拿到的訊息是「去寫收尾信，然後再來睡」——**一句明確暗示「什麼都還沒發生」的指示**。

## 兩個方向都要守，而我只守了一個

- ✅ 我守了：**附帶動作失敗不擋主動作**（關場炸了不擋下線）
- ❌ 沒守：**主動作失敗時附帶動作不該已經發生**

📌 前者是「回報層炸掉冒充主動作失敗」那族；後者沒有名字，而它更貴 ——
因為前者留下的是**沒做**，後者留下的是**做了一半而且帳是假的**。

## 而我把它放前面的理由是推論不是讀數

「關場那步已經不在線就結算不了」——**沒量過**。
今天量了：`Cmd_StreamWatch.SettleResidueAsync` 全函式對 `IsOnline` / `LockPath` **零命中**
⇒ 結算不依賴在線。⇒ **已證實的傷害贏過推測的傷害。**

## 動工時照做

附帶動作（清理／關場／通知）一律排在**主動作成功之後**。
真的必須排在前面時，要能回復 —— **不能回復就不要排在前面**。
