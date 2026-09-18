---
id: decision_d28-readonly-not-deleted
topic: senate-bank-rebuild
title: D28：舊帳本不刪轉唯讀，而選它的那條路整段移除（退路留在資料上）
type: decision
status: active
created_at: 2026-09-18
created_by: basecamp
links: []
related_docs: []
---

Tim 2026-09-18 拍板（逐字）：「**不刪，轉唯讀就好**」（問的是舊 `Treasury/ledger/` 那 14,000+ 筆分錄）。
落成 `Senate Docs/Logs/Decisions.md` 的 **D28**，並把 **D27（兩套並存）標記「已被 D28 取代」** —— ⛔ 不刪 D27，照 repo 既有慣例（`D11 … 已被 D13 取代`）。

## 這個拍板真正的形狀：**退路留在資料上，不留在程式碼上**

- **資料**：舊帳本與 `accounts/` 原封不動，狀態＝唯讀歷史。新銀行每一戶第一筆 `opening_balance` 帶 `source_ref` 指回它 ⇒ 刪掉會讓那個指標指向不存在的東西，而它仍是一個合法字串。
- **程式碼**：`money_authority` 旗標／`legacy` 分支／雙寫鏡像／雙寫期對帳器**整段移除**（TASK-0242 ④）。
  🩸 不留「以防萬一切回去」的理由：一個編得過、讀得到、呼叫得到的舊分支，會讓下一個人把錢寫進一本凍結的帳，而兩邊都是合法數字、**沒有任何一層會喊**。

## ⚠ 而有一格我刻意**沒有**動（下一個人不要以為是漏掉）

`bank_settings.json` 裡的 `money_authority` 鍵**留著**（資料層）。
理由：磁碟上還有四棵舊 checkout（`BarSubmodules` / `EOV` / `EmblemOfValor` / `LY`），它們跑的是會讀那個鍵的舊版 code ⇒ 刪掉鍵會讓那幾棵**靜默落回 legacy**。
⇒ 判準：**程式碼零命中是目標，資料鍵不是** —— 留一個無人讀的殘值，比讓別棵樹靜默改道安全。
