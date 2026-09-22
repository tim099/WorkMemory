---
id: pitfall_signature-vs-propagation
topic: agentcmd-queue-lost-update
title: 改了簽章不等於改了傳播；讀過那段 code 不會讓你看見旁邊那格
type: pitfall
status: active
created_at: 2026-09-23
created_by: kotoko
links: []
related_docs: []
---

**改了簽章不等於改了傳播；而「讀過那段 code」不會讓你看見旁邊那格。**

🩸 2026-09-22 同一支檔、同一天：
- `Save` 從 void 改成 bool（為了讓換檔失敗說得出來）
- 而 `SaveMerged` 照樣 `Save(...); … return true;` —— **回傳值被丟掉**，文件卻寫著「true ＝真的寫回去了」
- 同一天新寫的 `MutateQueueOnDisk` **接對了**那個 bool

⇒ 差別不是仔細不仔細：**新函式是「帶著那個 bool」寫出來的，舊的那支只是為了改守衛「被讀過」。**
📌 一般化：**讀一段 code 去改 A，不會順便讓你看見 B。**
改一個型別的簽章時，要去數**呼叫端**，不能靠「我剛剛讀過那個檔」。

⚠ 同族第三例：`Busy` 的訊息寫「等下一輪即可」，而 Watcher 只看 trigger 檔、
那一輪的 `finally` 把 trigger 清掉了 ⇒ **沒有人製造下一輪**。
行為不是新的（`Unreadable` 本來同形），**新的是那個承諾**。
⇒ 收成一句：**加一段話比加一段 code 容易，而話會被當成憑據。**
