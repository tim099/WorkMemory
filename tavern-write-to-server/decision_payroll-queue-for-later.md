---
id: decision_payroll-queue-for-later
topic: tavern-write-to-server
title: 發薪等不到銀行時排進它的 queue、不等
type: decision
status: active
created_at: 2026-09-25
created_by: basecamp
links: []
related_docs: []
---

**發薪等不到銀行那顆時：照正常協議排進它的 queue，不等（TASK-0297，Senate `46947b3`，Tim 2026-09-25 拍板模型）。**

- 為什麼不另造待補目錄：`ServerExecutor.Tick` 每個心跳都掃 `pending.trigger` ⇒ Server 起來前就排著的一筆，起來後下一個心跳自己接手 ⇒ Server 自己的 queue 就是那個「寫檔暫存」。一度做過 `pending_credits/`＋手動補送＋稽核標記，Tim 一句「跟正常 CLI 觸發一樣、不用處理重複」之後整套撤掉，沒進 commit。
- 只排四種：`autostart_timeout／autostart_failed／not_running／queue_busy` —— 全部發生在**送出之前**，排進去就是那一筆的第一次 ⇒ 不會重複、不用處理重複。
- ⛔ 不排：`timeout／unknown`（已送出、在 Server 手上，再排才是重複）、`build_mismatch`（刻意拒絕舊 exe）、`cmd_failed`（銀行明確拒絕）。
- 入口：`ServerDelegateCmd.ShouldQueueForLater`／`TryQueueWithoutWaiting`；呼叫點 `Cmd_TavernWrite.AppendPayroll`（`pay_queued` 計數）。
- ⛔ **訊息本身不排**：發文當下要回 seq 與 @ 通知；Server 拉不起來 ⇒ 整筆失敗、發文者看得到、自己重發。要不要改成也排由 Tim 決定（2026-09-25 還沒回）。
- 活體：停 `main`＋外部行程佔住 `_server_singleton.lock`（spawn 出來的那顆拿不到鎖 ⇒ 20s 後 `autostart_timeout`）⇒ Template 發文 ⇒ `pay_queued=1`、帳上 0 ⇒ 放鎖、拉起 main ⇒ 帳上恰好 1。⭐ 這個「佔鎖」手法是安全的重現法，**不要用改 exe 名**（見同主題 pitfall）。
