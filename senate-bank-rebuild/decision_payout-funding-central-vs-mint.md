---
id: decision_payout-funding-central-vs-mint
topic: senate-bank-rebuild
title: 請款分央行撥款／增發，補薪走增發（Tim 2026-09-22）
type: decision
status: active
created_at: 2026-09-22
created_by: basecamp
links: []
related_docs: []
---

**Tim 2026-09-22 拍板：請款要分「央行撥款」與「增發」兩種，而補薪走增發。**

- `central`＝央行 → 目標戶（走 `transfer`，**公庫變少**，總量守恆）
- `mint`＝**增發**（走 `credit`，⛔ 不碰任何帳戶，**總量變多**）

落點：`SCP_PayoutRequest.Funding`（空字串＝單子沒宣告）／`cmd bank op=approve --arg funding=`／
`BankAdminPage` 核准時兩顆鈕分開。優先序：本次裁決 > 單子宣告 > 預設 `central`。

## 判準（三條，每一條都有代價）

1. **預設維持 `central`，⛔ 不默默改成增發。** 改預設等於把舊單子的語意在無人知曉時換掉。
2. **讀取時 ⛔ 不補預設** —— 補了的話「開單人選了央行」與「開單人沒說」就同形，
   而裁決端要看得出後者（那代表這個決定被丟給它了）。⇒ 乾跑會印判定是**哪裡來的**。
3. **資金來源要印在金額旁邊**（清單、乾跑、審批畫面都印）——
   兩種錢在單子上長得一模一樣（都是「請款 N token 給 X」），而**對公庫的影響相反**。

## 🩸 為什麼會有這條拍板

補薪那筆錢是**勞動新產生的價值**，不是從公庫搬過來的。
而 2026-09-22 的 114 token 補發走了央行撥款 ⇒ 公庫平白少 114，
事後得再增發一筆 `payout_shape_correction` 補回去（Tim 選「只補公庫，不動大家的帳」）。

⚠ **代價要一起記**：那 114 則因此沒有逐則 `work_post` ref
⇒ 必須靠 `payroll_settled.json` 擋住補款工具，否則它會再發一次。
📌 ⇒ 一般形：**用 A 機制補 B 機制的帳，就會留下一批「B 的比對器看不見的錢」。**

## ⛔ 轉帳單不吃這一格

轉帳的語意就是 A→B 守恆，「增發」在那裡不成立 ⇒ 帶了直接 exit 2。

## 活體讀數（反向對照成立）

`funding=mint` 核准 1 token 給 `template`：`template` 46 → **47**、公庫 **21336 → 21336（沒動）**。
反向對照是同日稍早那 6 張 `central`：公庫 **21336 → 21222**。
⇒ 兩條路**共用同一把冪等鍵**（綁單號）⇒ 同一張單無論走哪一種都只生效一次。
