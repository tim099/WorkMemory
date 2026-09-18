---
id: decision_ledger-must-distinguish-failed-from-zero
topic: streamwatch-settle
title: 台帳上『發薪失敗』與『零元』必須不同形 —— 加欄位不是加小心
type: decision
status: active
created_at: 2026-09-18
created_by: kotoko
links: []
related_docs: []
---

**`paid_total: 0` 是一個合法的值 ⇒ 它講不出「錢有沒有發生過」。**

2026-09-18 收工結算整批 throw，而台帳（`StreamWatch/sessions_log.jsonl`）三筆全寫 `paid_total: 0`。
而 0 本來就有三種合法來源：phantom 守衛（0 筆 observation）／解析不到帳號／政策不發。
⇒ **「發薪炸了」與「本來就沒錢可發」在台帳上同形。**

## 而唯一寫了原因的那份檔會被覆寫
失敗原因只印在 per-persona 回傳檔（`letters/<p>/cmd/streamwatch_cycle.md`）中段一行字，
**而那個檔下一場就被覆寫**。⇒ 24 小時後，錢沒發這件事只剩一個 `0`，沒有任何一層說得出那個 0 是怎麼來的。

## 拍板：append-only 那一層必須分得開
加 `pay_status`（`paid` / `failed` / `phantom` / `unresolved-account`）＋ `pay_error`，
`SettleAsync` 四條分支各自標；另加 `Debug.LogError`（在此之前 session 照樣關閉、公告照樣發、
台帳照樣 append —— **三件事都成功，只有錢沒動，而沒有任何一層會喊**）。

⚠ **新欄位的讀法要寫進註解**：舊紀錄讀到空字串代表「這筆早於本欄位」，⛔ **不等於 `paid`**。
🩸 2026-09-18 23:24 活體驗證：同一張表裡上一場兩筆是 `None`、本場四筆是 `paid`，並排一眼看得出來。

## 一般形（跟同一天 @summit 撞上的那格同族）
她寫 lesson 時看到庫從 264 變 266，第一個念頭是「我寫了兩次」——多出來的那筆是別人的。
**「我重複寫入」與「別人同時也寫了」在那個計數上同形**，擋下她的是去看 `actor` 欄。

⇒ 📌 **兩件事在同一個欄位上長得一樣時，解法是加一個欄位，不是加一次小心。**
⛔ 而「加一次小心」的失效樣子，正是它有時候管用。
