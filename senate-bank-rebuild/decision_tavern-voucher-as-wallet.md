---
id: decision_tavern-voucher-as-wallet
topic: senate-bank-rebuild
title: 酒館券＝個人錢包：主動消費自動先扣券（白名單制，規則只住在 bank op=pay）
type: decision
status: active
created_at: 2026-09-18
created_by: basecamp
links: []
related_docs: []
---

## 拍板（Tim 2026-09-18）

酒館券是**另一本帳**（個人錢包），面額與 token 1:1，**主動消費時自動先扣券、不足的才扣 token**
（10 token 的消費可以是 3 券 ＋ 7 token）。
⚠ 我當時提過 A 案（券＝帳戶餘額的別名，沒有第二本帳），Tim 選 B。
⇒ B 案是可行的，因為**券是會被消耗掉的另一種資產**，不是同一個餘額的第二個說法。

## 三條設計判準

1. **規則只有一份，裝在銀行的付款那一步**（`senate cmd bank --arg op=pay`）。
   ⛔ 不讓每個呼叫端各判一次 —— 那會長出第二套政策而沒有人比對過。
   ⚠ 而它還有一個實際好處：Unity 那側**不可能**長出第二份名單（規則在 Server）。
2. **白名單，⛔ 不是黑名單**（fail-closed）。兩種錯的代價不對稱：
   漏列一個消費 kind ＝「券花不掉」（看得見、可補）；
   漏排一個系統費用 ＝「券被吃掉了」（看不見、補不回來）。
   ⇒ 名單目前：`canvas_pixel` / `sculpture_place` / `book_donation` / `book_tip`。
3. **先檢查兩邊夠不夠，不夠整筆不做**（Tim 拍板），⛔ 不部分扣款。
   ⚠ 兩次寫入之間**沒有補償**（Tim 明確不要）：先扣券再扣 token，
   token 那步失敗時券已經扣掉 ⇒ 錯誤訊息印出確切數字讓它能人工還原，⛔ 不假裝整筆沒發生。

## 命名：⛔ 券 id 不叫 `token`

叫 `tavern`。兩本帳的名字長得一樣的話，「錢包剩多少」與「戶頭剩多少」在畫面上就同形了 ——
而那正是同一天修掉的那隻病的形狀。

## 讀數上必須分開的一格

`pay_tavern` 一定要單獨印出來。
少了它，「3 券 ＋ 7 token」與「7 token」只差一個總數，**付款方式看不出來**
⇒ 券被吃掉就不會有人知道。
📌 同族：捐贈簿／打賞簿也補了 `paid_voucher` / `paid_token` ——
`tokens_spent: 6` 而帳本只扣 2，兩個數字各自都對，但單據上看不出來，
對帳的人會去找那 4 個不見的 token。

## 遷移的一格陷阱

舊酒館券帳（`ChatTavern/agent_bonus_quota.json`）的鍵是 **(bank, persona) 兩層**，
同一個人散在好幾個 bank 底下（apex-one 三個、summit 兩個、gura 兩個）
⇒ 遷移**必須跨 bank 加總**；只讀一個 bank 會靜默少算，而少算出來的數字完全合法。
