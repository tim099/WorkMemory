---
id: pitfall_write-back-eats-unknown-keys
topic: session-architecture
title: 寫回吃掉 model 不認識的鍵／[NonSerialized] 沒用／全域 Factory 污染測試 —— 三隻都回綠
type: pitfall
status: active
created_at: 2026-09-04
created_by: basecamp
links: []
related_docs: []
---

2026-09-04 做 TASK-0127 ④⑤ 時撞到的三隻。**共同形狀：寫入端安靜地少寫／多寫了東西，而每一層都回綠。**

## ① 寫回會吃掉「這個 model 不認識的鍵」（兩邊都中）

session 檔是**一人一檔位**，而同一份檔會被不同型別讀到：
管理頁與關場路徑讀 `UCL_SessionBase`（只有共通欄位），磁碟上那份還帶著各 kind 的專屬欄位。

🩸 活體：一份帶 `rounds`／`activity`／`activities_done` 的 FreeTime 殘留，
走「`Load<UCL_SessionBase>` → `Close` → `Save`」之後**三個欄位都不見了**，
而工具回報 `closed=1`、**沒有任何一層喊**。
⇒ **管理頁的「補收工」走的是同一條路** —— 它一直在做同一件事，只是沒有人回頭看那個檔。

**修法擋在寫入端（一個地方），不是叫每個呼叫端記得載入正確的子型別：**
- UCL：`UCL_SessionBase.RawJson`（載入時記原始 JSON）＋ `UCL_SessionService.MergeOntoRaw`
- SCP：`SCP_ActivitySession.Raw` ＋ `SCP_ActivitySessionStore.MergeOntoRaw`
- 只補「原始有、序列化結果沒有」的鍵，**不做刪除**（這一層不知道某個鍵消失是不是故意的）

📌 判準：**任何「讀成一個比檔案窄的型別 → 改幾欄 → 寫回」的路徑，預設都在吃資料。**

## ② `[NonSerialized]` 這套序列化器不看

修好 ① 之後我把 `RawJson` 標 `[NonSerialized]` —— 實跑後 session 檔裡真的長出一整包巢狀 `"RawJson": {...}`。
`JsonConvert.SaveFieldsToJson` 只跳過 **`[UCL_HideInJson]`** 與 multicast delegate。
⇒ **要排除欄位，用這套自己的 attribute，不要套別的框架的直覺。**
（而且這隻只有回讀整份檔才看得到 —— 只檢查「`rounds` 還在嗎」會全綠。）

## ③ 測試環境被正式環境的全域狀態污染

`SCP_ActivitySessionGatewayHost` 加了 `Factory`（全域，`Program.cs` 啟動就掛）之後，
selftest 的「這個 kind 沒有 handler ⇒ 明確降級」那條路**永遠走不到** —— 而那條路正是降級的保證。
⇒ `ClearForTest()` 要**連 Factory 一起清**。
📌 這一格是紅燈自己喊出來的，我沒有想到 —— 值得記的是形狀：
**加一個全域注入點的同時，就是在讓某條「沒有注入」的分支變成測不到。**
