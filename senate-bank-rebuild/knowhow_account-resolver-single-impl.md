---
id: knowhow_account-resolver-single-impl
topic: senate-bank-rebuild
title: 帳號解析收斂成一份：SCP_BankAccountResolver（TASK-0269）
type: knowhow
status: active
created_at: 2026-09-22
created_by: kaguya
links: []
related_docs: []
---

帳號解析（persona／agent／別名／帳號 → 正式帳號）**只剩一份實作**：
`SCP_Core/Runtime/Bank/SCP_BankAccountResolver.cs`。

退場的兩份（TASK-0269，2026-09-22）：
· `UCL_TreasuryAccountResolver.cs`（645 行，Unity）⇒ 刪。Unity 呼叫端改走
  `UCL_BankResolve`（**零邏輯宿主轉接**，只供 letters／data／region 三個根）。
· `_lib/bank_resolver.py`（292 行）⇒ 刪。python 走 `senate cmd bank-resolve`。

接手的人要知道的三格：

① **權威是 `letters/<persona>/bank/<region>.md`**，registry 只補 system_accounts／
   closed_accounts／已退居 legacy 的 `agent_banks`。⛔ `bank_personas` 反向表 2026-09-07
   已退出解析，今天還有 2 筆是舊值（`Sirius`：FRS vs Federal Reserve System；`kotoko`：Spectre vs cc）。
   ⇒ 要「這個帳戶底下有誰」走 `GetBoundPersonas`（**由正向導出**），⛔ 不要讀 registry。

② **帳本的 `account_id` 是小寫，綁定檔是原拼法**，而綁定名單是 Ordinal 比對
   ⇒ 拿帳本字串直接查綁定會**回一個看起來很合理的空名單**。
   先 `Resolve()` 歸一再查。（2026-09-22 保管費轉券 preview 第一次跑就中，5 個帳戶有 3 個報「沒有 persona」。）

③ **解析器刻意不掃 `Treasury/accounts/`** —— 帳戶檔是「後台開過戶」的產物，不是「這個名字可以收錢」的宣告。
   `bank-audit` 掃它是對的（它算帳號宇宙），解析器不掃也是對的（它要權威）。同一個目錄兩個問題。

⚠ 改這支之前：對拍的作法是 `senate ucmd run Invoke` 逐位問舊實作 ＋ harness 問新實作，
pool 全體逐位比。2026-09-22 那次 31/31 相同，而**紅過兩次，兩次都是夾帶的「改良」**。
