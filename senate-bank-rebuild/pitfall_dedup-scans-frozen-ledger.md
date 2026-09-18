---
id: pitfall_dedup-scans-frozen-ledger
topic: senate-bank-rebuild
title: 權威切換會讓所有「掃舊帳本判重」的保護安靜失效 —— 而編譯器是唯一讓它們現形的東西
type: pitfall
status: active
created_at: 2026-09-18
created_by: basecamp
links: []
related_docs: []
---

## 症狀：權威切到新銀行之後，**所有「掃舊帳本判重」的保護安靜失效**

它們不跟權威旗標走（`LoadAllEntries` / `LoadEntriesAfterDate` / `Audit` / `FindDuplicateByIdempotencyKey` 都是無條件讀舊 `Treasury/ledger/`）。
⇒ 切換後它們掃的是一本**凍結**的帳 ⇒ **永遠找不到**，而「沒找到」與「我看錯帳本」在回傳上**完全同形**。

實際踩到的兩處（都動錢）：
1. **保管費**（`UCL_BartenderDaemon`）：`alreadyChargedToday` 從舊帳本建 ⇒ state 一失效就會**重複扣款**，而第二層保護已經死了
2. **觀影結算發薪**（`Cmd_StreamWatch.AlreadyCredited`）：同族，⇒ 可能**重複發薪**

## 修法（TASK-0242 ⑭，`UCL_Core 434f98b3`）

- **判重的唯一權威改成 Server 的 `idem_key`** —— 兩處的兩隻腳都帶上（保管費 debit ＋ 央行入庫 credit）
- 帳戶清單改問新銀行的 `GetAllBalances()`（它本來就是那一輪要的東西）
- ⭐ **「扣了多少」改成扣完回讀 before/after 相減，⛔ 不用算的** —— 冪等命中時兩者相等，
  於是「這次沒有動錢」是**量出來的**不是推的
- 把舊帳本的讀取端抽成 `UCL_TreasuryHistory`（名字就說出它是哪個時代的答案），
  `audit`/`verify` 改問它且**回傳檔印定語**：「資料源凍結於 2026-09-18，表格為空不代表沒有交易」

## ⭐ 而讓我看見這一族的不是警覺，是**編譯器**

移除舊實作之後紅了 **19 個** CS 錯誤，**全部**是這些讀取端。
⇒ 判準：**要讓一族隱形的相依現形，就把它們依賴的東西刪掉，讓它們編不過** ——
⛔ 比 grep 可靠，因為 grep 找得到名字、找不到語義。

## 驗收讀數

- 冪等反向對照：同一把 `idem_key` 送兩次 ⇒ `148 → 146` ／ `146 → 146`，新帳本只多一筆
- ⛔ **保管費那條路本身沒有活體讀數** —— 它要跨 UTC 日才跑；我驗到的是它底下那層
