---
id: pitfall_new-state-chase-decision-layer
topic: agentcmd-queue-lost-update
title: 新增狀態後追到做決策的那一層，不是呼叫端
type: pitfall
status: active
created_at: 2026-09-27
created_by: kaguya
links: []
related_docs: [task:TASK-0265, commit:0bfad45b]
---

**新增一個狀態之後，要追的是「拿回傳值做 if 的那一層」，不是呼叫端**（TASK-0265 QA 第二輪，2026-09-27）

我替 GetBankAccount 新增 `BankSourceUnreadable`、改了所有**直接呼叫端**，而一次碼審抓到 8 格漏網 —— 每一格的直接呼叫端都「處理了新狀態」，壞的是再上一層仍用舊的二分法（有值／空字串）做決定：
- `Cmd_PersonaProfile migrate_bank`：`hasOwn = curSrc == currency` ⇒ unreadable 被讀成「沒有本區綁定」⇒ 覆寫
- `UCL_PersonaProfile.RenameAgent`：`bindHit` 判 false ⇒ 漏改一位而回 success
- Resolver：不落快取但**本次照答** ⇒ 掉到大小寫歸一，錢進別人的帳戶
- `RateAdminPage`：Load 失敗回的是**空設定不是 null**，工具列只擋 null ⇒ 存檔鈕照樣能把空設定寫回（這格舊版就有）

⇒ grep 的受詞：**讀 `oSource`／回傳值做判斷的地方**（`== currency`、`.Length == 0`、`!= null`），⛔ 不是「呼叫這支函式的地方」。
⭐ 抓到它的是一個**不帶我假設**的獨立碼審 agent，不是我更仔細。
另一格同族：條文 ① 的母體是**寫入端**，我從讀取端倒推、又只看同一個檔 ⇒ 14 個寫入端的讀取端住在別的檔而漏掉，而我寫了「量不到 0」。
