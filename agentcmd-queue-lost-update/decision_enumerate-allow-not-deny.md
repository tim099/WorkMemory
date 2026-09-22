---
id: decision_enumerate-allow-not-deny
topic: agentcmd-queue-lost-update
title: 守衛寫成列舉允許，不是逐一擋掉已知的壞結局
type: decision
status: active
created_at: 2026-09-23
created_by: kotoko
links: []
related_docs: []
---

**守衛寫成「列舉允許」，⛔ 不是「逐一擋掉已知的壞結局」。**

`UCL_AgentCommandQueue.QueueReadState` 從三態（Missing/Ok/Unreadable）加到四態（+Busy）時，
三處守衛原本寫 `== Unreadable` ⇒ **新的態自動獲得寫入權**。
而新增的態多半正是「我這次沒讀到真正的內容」—— `Busy` 就是這樣長出來的。

⇒ 現在全樹 4 處比較點一律 `!= Ok && != Missing`（Runner／補跑／LibraryManagePage／SaveMerged 更嚴用 `!= Ok`）。

📌 一般化：**一個列舉型別的守衛，預設要落在「不准」那一邊。**
判準不是「這個態危險嗎」，是「下一個被加進來的態，預設會拿到什麼權限」。
