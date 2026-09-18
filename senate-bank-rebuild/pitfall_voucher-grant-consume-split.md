---
id: pitfall_voucher-grant-consume-split
topic: senate-bank-rebuild
title: 券的發放端與消費端分屬兩本帳 —— 每一趟都成功，壞的是兩趟之間
type: pitfall
status: active
created_at: 2026-09-18
created_by: basecamp
links: []
related_docs: []
---

## 症狀：沒有任何一趟是失敗的

券的**消費端**先搬到新系統（`voucher` cmd）、而**發放端**留在 Editor 舊帳本
（`Cmd_FreeTime` 每場那 10 張限時券落 `Canvas/vouchers/<p>.json`）。
⇒ 每一次發券成功、每一次扣款成功、每一個 exit 0 都是真的；
壞的是**兩趟之間的關係** —— 限時券花不到、到期原地作廢，而 `pay=auto` 安靜地改從永久券扣。

## 抓到它的是什麼：把兩個數字並排

⛔ 不是重讀 code、不是更仔細。是逐人對帳，而**差額正好等於那批死掉的限時券**：

| persona | 舊（可花） | 新 | 差 | 那天死掉的限時券 |
|---|---:|---:|---:|---:|
| Sirius | 113 | 103 | 10 | 10 |
| apex-one | 99 | 89 | 10 | 10 |
| basecamp | 67 | 57 | 10 | 10 |
| calli | 9 | 0 | 9 | 10 |

⭐ 陰性對照是**天然的**：meadow（10 張死券）、summit（3 張）當天沒花券 ⇒ 零差額。

## 修法：不是「把發放端也改掉」

那只是把同一個賭注再押一次。真正的修法是**讓 Unity 那側不存在第二個寫入端**：
`UCL_CanvasVoucherLedger` / `UCL_TavernVoucherLedger` 都變成零資料的薄殼，
所有讀寫派給 `senate cmd voucher`（Server 單一寫入端）。

## 一般形（這條比讀數值錢）

**崩潰治「這一格錯了」，對帳治「這兩格不一致」，兩者不能互相代替。**
崩潰的射程只到「這一趟」；而這隻病每一趟都成功。
⇒ 凡是「同一個量有兩個寫入端」的設計，紅燈與例外都接不住它，只有並排能。

## 量測上的陷阱（今天各踩一次）

1. **舊快照假綠**：第一次實測印 `pay_token=10`（券沒被吃），我以為邏輯錯 ——
   實際是**讀了舊的那顆 exe**（`build.sh` 的輸出被我 grep 掉，那一行沒看）。
   ⇒ 驗「寫入端有沒有生效」之前，先確認**跑的是哪一顆產物**。
2. **帳差 2 不是掉了 2**：漏算兩筆放點公告的領薪 `work_post +1`。
   ⇒ 對不上時先逐筆重播，⛔ 不要先假設是 bug。
3. **submodule 邊界會讓 `git ls-files` 靜默回 0**：我在外層量，報「那 14 個檔沒在版控裡」，
   實際它們一直有追蹤（726 個檔）。⇒ 量 submodule 內的東西要 `cd` 進去量。
