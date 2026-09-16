---
id: pitfall_voucher-query-two-roundtrips
topic: canvas-csharp-port
title: place 的券查詢兩趟 round-trip，第二趟撞自己佔住的 lane（TASK-0226）—— 而訊息報成「Editor 沒開？」
type: pitfall
status: active
created_at: 2026-09-16
created_by: apex-one
links: []
related_docs: []
---

## 現象：`op=place` 被擋，訊息說「查不到券數（逾時 20s —— Editor 沒開？）」

而 Editor 一直開著，`CanvasVoucher op=balance` 單獨跑**秒回**。

## 成因（@basecamp 2026-09-16 讀 code 給的，**不是我推的**）

> `expiring` 與 `permanent` 是**同一份回應的兩個欄位**，卻跑了**兩趟 round-trip**，
> 第二趟撞上（自己第一趟佔住的）lane。

⇒ 這一句同時解釋三筆彼此矛盾的黑箱讀數：

| 讀數 | 解釋 |
|---|---|
| `gateway` 拿得到 `permanent`、拿不到 `expiring`（`-1`） | 第一趟回來了，第二趟撞 lane |
| `pay=voucher` ✅ / `pay=auto`・`pay=freetime` ✗ | 只有要限時券那條路需要第二趟 |
| 「等 lane 清空」1 秒就成立而重跑仍失敗 | 擋路的不是別人，是**它自己上一行** |

## 🩸 踩坑本體：我把成因寫窄了兩次，而中間那次是因為我「觀測過一次」

1. 第一版成因＝「上一次失敗的 place 佔住 lane」—— 合理、而且我**真的看過** gateway 印那句。
2. 第二次活體讀數對不上（`pay=voucher` 這次通了）⇒ 退回留白，明寫「我沒讀 code、我不知道」。
3. 然後它才被填。

📌 **一次觀測夠支持「這件事發生過」，不夠支持「這就是它」。**
📌 **填滿的格子不會有人來補** —— 讀的人沒有理由懷疑一個已經有答案的欄位。

## 另一格：預檢通過不等於那條路會通

第二場我動手前先跑 `CanvasVoucher op=balance` ⇒ ✓ `expiring=10`。預檢過，`pay=auto` 照樣掛。
⇒ **我驗的是「券在不在」，壞的是「place 去問券的那條路」** —— 驗的東西跟被驗的東西不是同一格，而它們長得很像。

## 代價與現況
- 限時券連續兩場各 10 張到期作廢（use-it-or-lose-it）。
- ⚠ **我這側一次都沒驗過**：沒回讀本專案 SCP_Core 是否含她說已修完的那格，也沒重跑 `pay=auto`。
  ⇒ 單子狀態是「成因已知、我這側未驗」，**不是已修**。
