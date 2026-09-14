---
id: decision_ledger-shape
topic: senate-bank-rebuild
title: 帳本三格刻意與舊系統不同：不存 balance_after、只鎖 debit、冪等先於餘額
type: decision
status: active
created_at: 2026-09-14
created_by: basecamp
links: [identity-account-unification]
related_docs: [commit:8b55bc2, task:TASK-0209]
---

**帳本形狀三格，每一格都是「刻意跟舊系統不一樣」**（`SCP_Core/Runtime/Bank/SCP_BankLedger.cs`）

1. **⛔ 不存 `balance_before` / `balance_after`。**
   舊系統每筆都蓋，而它們是**去正規化的冗餘**（真相是重放求和），併發時會蓋一份過期快照
   —— 說謊而沒有任何一層會喊。
   ⭐ 而拿掉它之後，「鎖要不要涵蓋 credit」那個取捨（我原本列成甲／乙給 Tim 挑）**整個消失了**：
   **credit 不讀餘額，就不必排隊。** 📌 一般形：**去掉一個欄位，可以消掉一個設計難題。**
2. **臨界區只有 debit 的「讀餘額 → 比大小 → 落檔」**，整段在同一把 in-process lock 內。
   ⚠ 它只在同一個 process 內有效 —— 跨 process 靠「只有 Server 寫」那個**前提**。
   本層擋不住有人在 Server 之外呼叫它；真正的閘在 `Cmd_Bank : ServerDelegateCmd`。
   ⛔ 註解裡把這件事寫明，不假裝那個前提是自證的。
3. **冪等鍵先於餘額檢查。** 反過來的話，重複請求會在餘額剛好不足時噴「餘額不足」——
   而那句話是**假的**（錢早就扣過了），它會讓人去查一個不存在的餘額問題。

**帳號 id（`SCP_BankId`）**：全小寫 `ToLowerInvariant`（⛔ 不是 `ToLower` —— 跟文化走，
土耳其文的 `I` 會變 `ı`，同一個 id 在不同機器上會正規化成兩個）；
**不合法就拒絕，⛔ 不替呼叫端換字元** —— 換字元＝靜默把兩個帳號併成一個，而那是錢。
正規化**落在寫入端**：讀取端各自 case-insensitive 比對的話，漏掉的那個查不到，
而那跟「這個帳號不存在」同形。

**開戶是顯式動作**（`SCP_BankAccount`）：舊系統讓帳號在 ledger 裡自己長出來
⇒ 打錯一個字就憑空多一個帳號而錢真的進去（實測：51 個有餘額的帳號裡 **42 個沒開過戶**）。
銷戶是**狀態不是刪檔**；認不得的 `status` 走安全側當 closed
—— 反過來的話，一個壞欄位會讓帳號變成**可以動錢的**。

**`bankRoot` 是 Stored 不是 Derived**（Tim 2026-09-14）：跨專案共用同一套。
⛔ 刻意沒有 auto 推導 —— 能推的只有「本專案底下」，而不要跟著專案漂正是它存在的理由。
留空 ＝ 宿主預設 `<Senate repo>/SenateData/Bank`（該層已 gitignore ⇒ 錢不跟著 code 進版控）。
