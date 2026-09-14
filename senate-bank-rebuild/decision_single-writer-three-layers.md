---
id: decision_single-writer-three-layers
topic: senate-bank-rebuild
title: 單一寫入端由三層撐著，而 A2 單例鎖必須先於 A4 自動啟動
type: decision
status: active
created_at: 2026-09-14
created_by: basecamp
links: [senate-backend]
related_docs: [task:TASK-0209, commit:c57936a, D:/Unity/Senate/src/Senate.Core/ServerHost.cs]
---

**「單一寫入端」是一個前提，而它由三層撐著 —— 而那三層有順序。**

| 層 | 它防什麼 | ⚠ 沒有它會怎樣 |
|---|---|---|
| **A2 單例鎖**（OS advisory lock，`_server_singleton.lock`） | 兩顆 Server 同時活著 | 實測：拿掉鎖，4 顆同時 start **4 顆全部登記成功** |
| **A4 自動啟動** | 委派 Cmd 撞到沒 Server | —— |
| **A6 排乾 (乙)** | 收工時切掉跑到一半的 lane | 被切的那條下一顆 Server 會**翻回 pending 續跑** ＝ 那筆 cmd 做第二次 |

🔴 **順序不可換：A2 必須先於 A4。**
自動啟動一旦上線，N 顆 CLI 同時發現「沒在跑」就 N 顆一起 spawn ——
**那正是 A2 要擋的**，先做 A4 等於自動製造我們要防的競態。

**A2 為什麼是 OS lock 不是 pid 鎖檔**：差別只有一格 —— **process 死掉 OS 自動放**。
pid 鎖檔要靠程式記得刪，當機／強制關掉就留死鎖，而銀行搬進來後被鎖住的是
跨日保管費／領薪／發文計酬那些**沒有人在看**的自動流程。
📌 職責切開一句話：**「誰是唯一那顆」由鎖回答，「那顆是誰」由 registry 回答。**
⇒ `Register(iAllowMultiple: true)` 原樣不動 —— 它本來就不是互斥機制，不該假裝是。

**A6 為什麼拒絕退出而不是硬切**：續跑＝重做一次，而它可能是扣款。
🔴 而 (乙) 有一個**不做就更糟的連帶**：`Stop()` 原本等 5s 就 kill，排乾上限 30s
⇒ **排乾會被自己的 stop 指令硬切掉**。改成「它還在跳心跳就繼續等」。
⚠ 「還在排乾」與「卡死了」在 registry 那層**同形**（都是 Alive），分得開它們的只有心跳新鮮度。

**A1 獨立 exe**：`Senate.Server` 只參照 `Senate.Core`，產出 1.2MB／0 個 GUI 檔
（對照 `Senate.Cli` 16MB／10 個）。⚠ **打包還沒接**（build.sh／publish／spawn 目標），
那裡有「再多 70MB self-contained」的取捨等 Tim。
