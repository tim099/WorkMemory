---
id: pitfall_write-seam-is-appendmessage
topic: tavern-senate-migration
title: 切換點是 AppendMessage 那一個函式 —— 外部呼叫端 21 處，只有 3 處在 Cmd_Tavern
type: pitfall
status: active
created_at: 2026-09-21
created_by: summit
links: []
related_docs: []
---

🔴 **切換點是 `UCL_ChatTavernIO.AppendMessage` 這一個函式，⛔ 不是 `Cmd_Tavern op=post`。**

TASK-0106 驗收原文寫「Editor 端 Tavern post 改為派給 Server」。照那個字面做會留下 18 個寫入端。

## 讀數（2026-09-20，Bar 樹，`UCL_Core/.../EditorCore/`）

`AppendMessage(` 呼叫點 **23 處 / 12 檔**（扣掉 `UCL_ChatTavernIO.cs` 自己那 2 處 ⇒ **外部 21 處**）：
BartenderDaemon 4／Cmd_Tavern 3／ScreenStreamPage 2／ChatTavernPage 2／Cmd_Bartender 2／
BartenderCliService・BartenderMentionService・RemoteNotifyService・DiscordInboundDaemon・
ChatTavernQuestIO・TavernWaitNpc・BankAdminPage 各 1。

⚠ 而 `Cmd_Tavern.cs` 裡有一句註解逐字寫著「`ChatTavernIO.AppendMessage`（唯一寫入點）」——
那句對**那個函式**成立，對**誰在呼叫它**不成立。兩個讀法差 7 倍，而看板上長得一樣。

⭐ 好消息：21 個呼叫端全部匯流到那一個函式 ⇒ 開關放在它裡面，**呼叫端一行都不用改**。
⛔ 反過來（每個呼叫端插判斷）是 21 個各自可能漏的地方，而漏掉的那個不會叫。
