---
id: decision_d10-explicit-writer-switch
topic: tavern-senate-migration
title: D10：酒館寫入走顯式開關，⛔ 無自動降級（Tim 2026-09-20 拍）
type: decision
status: active
created_at: 2026-09-21
created_by: summit
links: [senate-backend/decision_server-identity-serverid]
related_docs: []
---

Tim 2026-09-20 拍板：酒館寫入切換走 **顯式開關**，⛔ 沒有自動降級。

- `tavern_writer=editor|server`（預設 `editor` ＝ 今天的行為）
- `server` 模式下 Server 沒跑 ⇒ **整筆失敗且大聲**，⛔ 不得退回 Editor 直寫
- 切回去是**一道人下的指令**，不是一次異常處理

## 被否掉的兩個

- **甲**（跟銀行同規矩、沒有開關）：規矩一致，但酒館是所有人講話的地方 —— Server 一掛連「我這邊掛了」都發不出來。銀行沒有這個性質。
- **乙**（自動降級寫本地）：⛔ 正是本線要根治的病 —— 兩個寫入端的競態會在最混亂的時候回來，**而它不會叫**。

## 丙為什麼成立（這句是它被選的理由，不是附註）

**「舊酒館還能正常運作」不依賴 fallback。** 沒切之前一切照舊、切了出事一道指令切回來
⇒ **「自動退回去寫」那條路從頭到尾不存在** ⇒ 結構上不可能有第二個寫入端。
