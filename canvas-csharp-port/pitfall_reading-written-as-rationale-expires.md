---
id: pitfall_reading-written-as-rationale-expires
topic: canvas-csharp-port
title: 把會過期的讀數寫成決策理由 —— Editor 閘「沒有消費端」那格，2026-09-08 換了擋著的理由
type: pitfall
status: active
created_at: 2026-09-08
created_by: basecamp
links: []
related_docs: []
---

**2026-09-08 重量：decision_two-layer-gateway 的附帶決定 2 的前提，有一半過期了。**

那條 decision 寫「Editor 端 gateway 刻意不做：Unity 那側零個 .cs 引用 `SCP_CmdRegistry` ⇒ 沒有消費端」。
今天逐格重量，**判定不變但理由要換**，接手的人別照舊句子推：

| 讀數 | 2026-09-03 | 2026-09-08 |
|---|---|---|
| Editor 端有沒有 SCP_CMD 執行入口 | ❌ 零個 .cs 引用 | ✅ **有** —— `Cmd_FreeTimeActivity.RunCmdStep`（TASK-0143 09-07 落地，`Find` ＋ `Dispatch` in-process）|
| 有沒有活動 md 宣告 canvas 走 `cmd_steps` | —— | ❌ **沒有**（全樹只有 `book-writing` / `reading` 兩份，都不是 canvas）|
| `SCP_CanvasGatewayHost.For(<LY 資料根>)` 在 Editor 內 | 未量 | **null**（`ucmd run Invoke` 反射，Editor log `OK (void / null)`）|

⇒ **判準沒變（「它現在有沒有第一個消費端」），變的是哪一格在擋**：
從「**沒有橋**」收窄成「**橋有了但沒有人走**」。

📌 為什麼這一格值得記在記憶而不只是單子上：
那句「零個 .cs 引用 `SCP_CmdRegistry`」是**一個會自己過期的讀數**，
而它被寫成了決策的理由 ⇒ 下一個人讀到它時，它看起來仍然像一條判準。
**判準不會過期，讀數會** —— 拿讀數當判準寫，過期時不會有任何一層喊。

⚠ 而 `For()` 回 null 是**安全的空**不是洞：介面契約（`SCP_ICanvasGateway.cs`）寫死
「沒裝上時 `For` 回 null，呼叫端**必須 fail loud**，⛔ 不准 fallback 到假裝付過款的實作」
—— 所以現況不會有「像素落盤而錢沒扣」那種帳。要動它之前先讀那段註解。
