---
id: decision_transfer-atomicity-in-write-end
topic: senate-bank-rebuild
title: 轉帳的原子性放寫入端（op=transfer ＋ 回捲），⛔ 不在頁面上拼
type: decision
status: active
created_at: 2026-09-17
created_by: gura
links: [senate-gui-bridge/pitfall_pinned-scope-and-probe-subject]
related_docs: [commit:def67e9, src/Senate.Core/Cmd_Bank.cs, src/Senate.Cli/Pages/BankAdminPage.cs]
---

## 拍板：轉帳的原子性放**寫入端**（`Cmd_Bank op=transfer`），⛔ 不在頁面上拼

Tim 2026-09-17 要銀行後台加「開戶／打款／轉帳」三格。而 `cmd bank` 當時**根本沒有 transfer**
（只有 accounts / open / balance / credit / debit / close / reopen）。

⛔ 沒有在頁面上用「扣款 + 入帳」兩次派遣拼一個轉帳，理由：
**帳本沒有交易** ⇒「兩腳都成功」不是天生的，是**有人負責**的。
拼在頁面上的話，每一個呼叫端都得自己寫一次回捲，而**漏寫的那一個不會報錯** ——
它只會讓錢停在半路，而兩邊的餘額各自看起來都正常。
⇒ 守恆是寫入端的性質，不是使用者的紀律。這也正是本 Cmd 是「單一寫入端」的理由。

## 實作要點（`Senate def67e9`）

- `account` ＝轉出方、**`to_account` ＝收款方另開一格**
  ⚠ 一格裝兩個角色的話，「我填的是誰」要靠 op 才讀得出來，而錯填的代價是錢進了別人的帳。
- 兩腳共用 `tx_id`，冪等鍵各自是 `<tx>/out`、`<tx>/in`、`<tx>/rollback`
- 收款腳失敗 ⇒ **回捲**轉出腳（`kind=transfer_rollback`）
  ⛔ **不刪掉轉出那一筆** —— 帳本 append-only，「沒發生過」與「發生了又撤銷」是兩件事，
  抹掉前者會讓事後查帳的人看不到這裡出過事
- 回捲**也**失敗 ⇒ exit 5，印兩戶餘額 ＋ **人工補救的完整那一行指令**，⛔ 不靜默吞掉
- 自己轉給自己**擋下**（比對不分大小寫）：只會留兩筆相消分錄、餘額不變
  ⇒「我轉錯對象」與「我轉給自己」事後長得一樣，而前者要追、後者不用
- 呼叫端沒給 `idem_key` 時現生一個，**並在輸出講明「重送會再轉一次」**
  ⇒ 冪等與否是讀得到的事實，不是要人記得的細節

## 讀數（隔離暫存 bank_root 實跑，⛔ 沒碰真帳）

種子 t-alice 100 ／ t-bob 0

| 情境 | 結果 |
|---|---|
| 轉 30 | ✓ 70 / 30，合計 100 |
| 自己轉自己（`T-ALICE`） | 🔴 擋 |
| 餘額不足 999 | 🔴 轉出腳失敗，**整筆沒有發生** |
| **收款方沒開戶** | 🔴 **已回捲**；回讀 70 / 30、**合計仍 100**、orphan 0；分錄看得見 `transfer_rollback` |
| 同 `idem_key` 轉兩次 | ↻ 第二次判重，60 / 40 不變 |

⭐ 第四格是唯一真正重要的：**回捲那條路真的被走到了**，
而判準是**回讀帳本的合計**，不是它自己印的那句「已回捲」。

## ⚠ 仍然沒有驗到的那一格（照實標）

**真帳上按下二段確認的第二段「送出」從來沒跑過。** 頁面只驗到 arm
（點一次 ⇒ 只變成「⚠ 再按一次確認轉帳 Luna → cc 5」，回讀 `Luna` 287 → 287 未動）。
⇒ 要補就拿 `Template` 帳戶實跑一次，判準是**回讀兩戶**餘額
（守恆是兩個數字的**關係**，⛔ 不是其中一個數字的性質 —— 這也是 `Describe2` 存在的理由）。

## 📌 頁面層的一格機械限制（會咬人）

`SCP_Ui.Fold` **收合時不建子節點** ⇒ 開戶／打款／轉帳三格**各自帶自己的欄位**，
⛔ 不共用一份署名三欄（kind/ref/caller）。共用的話，收起放欄位的那一格會讓另一格送出時
**靜默少三欄**，而少的正是 `cmd bank` 會擋的那三欄 —— 失效樣子是「按了沒反應」。
