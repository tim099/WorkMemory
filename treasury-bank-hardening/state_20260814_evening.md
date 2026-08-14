---
id: state_20260814_evening
topic: treasury-bank-hardening
title: 2026-08-14 晚間收檔 — 帳號解析全線 + python 直寫旁路封閉
type: state
status: active
created_at: 2026-08-14
created_by: summit
links: [treasury-bank-hardening/state_20260814_account_resolution]
related_docs: []
---

**2026-08-14（summit wake#51）晚間收檔 — 帳號解析全線 + 旁路封閉**

⚠ 更正早上那份 state 的數字：早上寫「35 個孤兒 / 2616 token」是當時實查；
Tim 當天陸續歸戶並銷戶（Zeta 310→zeta、antigravity 16→a、Antigravity 95→a），
`tavern-keeper` 復戶並補登記為 system_account。**現況 32 個孤兒**（數字會再變，動手前自己數）。

**今天全部落地並驗過（依時序）**
1. `UCL_TreasuryAccountResolver` 六段解析 + SelfTest 六條不變式（58 項全過，含撞名清單）。UCL_Core 1f3c506。
2. Credit/Debit 接上解析 + `resolveAccount` 開關。**判準：從既有帳號清單選出來的＝認字面；從身分推導的＝歸一。**
   歸戶轉帳三處認字面 —— 實測 Antigravity 95→0 / a 178→273，總量守恆（若讓解析介入會變成扣正主的錢）。
3. BankAdminPage：👻 孤兒帳戶（標記遷移／銷戶三閘）＋🧭 帳號解析規則（試算／別名／補登記／復戶）。
   ⚠ 銷戶後名單刷新的過濾要做在**掃描裡**（孤兒名單由 ledger 推導而銷戶不刪 entry）；
   例外：已銷戶卻餘額≠0 不隱藏還標紅。
4. 酒館發文身分收斂成 persona（c103e1f）→ 沒帶 persona 改匿名不擋（862fc68，Tim 抓到我規則打架）。
5. **封掉 python 直寫 ledger 的旁路**（f112909）：`fire_salary_credit` 改走 op=credit；
   `bank_resolver` Step 4 的 derive（孤兒製造機）改成原樣回傳 + fail-loud；刪 `_lib/treasury_ledger.py`（零 importer）。
6. `RunBrief` 補新鮮度驗收（0252d13）：mtime 必晚於本次執行起點。紅路徑用 timeout=1ms 重現 wake#49。
7. 自動通知重編關閉（9761185）：**兩層根因** —— 裸 static + `LoadConfig` 讀 `enabled` 與新加的
   PersistEnabled 兩套持久化打架。收斂成單一來源（EditorPrefs）。第二層是 Tim 看畫面抓的。
8. catchup：`--limit`/`--full` 移走「需要 | head」的理由（680f323）→ 再修 `--limit` 取最舊 N 筆、
   cursor 只推到已顯示的最新一筆（6b92f96）。**沒顯示的不算已讀**，與 EPIPE 那條統一。
9. 文件：新增 `Workflows/Treasury_Account_Consolidation_Workflow.md`（跨專案 SOP，含地雷 #8
   「問還有誰能寫 ledger，不要問我的解析寫對了嗎」）；Cmd_Tavern.md / ChatTavern_Workflow.md /
   Commit_Workflow.md / skill 四 target 同步（只寫 persona，不提舊格式）。
10. glossary 新詞 `恰好綠`（coincidence-green），含 Sirius 的「怎麼被使用」一節。

**apex-one 平行完成（會撞車的部分已協調）**
`UCL_CmdArgsValidator`（ArgsSpec 第一次有人執行，65cdd7b）＋別名表七份收成一份（9c11ffb）。
Cmd_Tavern.cs / Cmd_Treasury.cs 由她收，我工作區淨空讓路。

**pending**
1. 「📝 標記遷移」按鈕已由 Tim 實點過（Antigravity 那筆）；32 個孤兒仍待逐個處理。
   `agent_aliases` 仍空 —— 認不出的（zeta-bank / discord:xxx / subconscious-daemon）要逐個決定
   「加別名搬走」或「補登記原地承認」，二選一不是兩個都做。
2. bank_resolver 移除剩兩步：canvas.py + registered_mail.py 改傳 persona；awakening.py 離線顯示 bank 要不要留（要拍）。
   ⚠ 殘留定義域差異：Python `resolve_bank_account` 只吃 agent、C# `Resolve` 吃全部 —— persona 餵進前者仍生孤兒。
3. Cmd_Tavern 其他 op（inbox_read / task_next / session_enter）Required 仍是 agent，是否一併收斂待評估。
4. catchup `--limit` 的異源驗收由 apex-one 執行（她已同意，四步協議）；EPIPE 整合層未驗（輸出 5.5KB < 管線緩衝 64KB）。
5. FreeTimeAdminPage 欠 Docs~/{lang}/UCL_EditorPage/ 文件與 index 回填。
