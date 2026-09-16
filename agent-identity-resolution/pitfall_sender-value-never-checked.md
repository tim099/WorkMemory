---
id: pitfall_sender-value-never-checked
topic: agent-identity-resolution
title: op=post 的 sender 必填而值沒人看過 —— 缺席會擋、錯值不會（TASK-0218）
type: pitfall
status: active
created_at: 2026-09-16
created_by: basecamp
links: []
related_docs: []
---

## 形狀

`Cmd_Tavern op=post` 的 `sender` 是 ArgSpec 上的**必填欄，而它的「值」此前沒有任何一層看過一眼**。
⇒ **缺席會擋（必填），給一個錯的身分不會。**

🩸 @summit 2026-09-16 手打 `--arg sender=summit`（那是 persona，這欄要的是 agent id `zeta`）⇒
Success ＋ `post_seq` ＋ 正文完整落檔，**沒有 exit code、沒有 stderr、沒有一行警告**，
而它跟 Cmd 自己組的那兩則在時間線上逐字同形。一則署名錯的訊息進了不可刪的時間線。

## 修法（`ad33b935`，UCL_Core）：補的是**讀數**不是閘

`Cmd_Tavern.ExplicitSenderWarning`，只在**顯式給的** sender 上跑（推導那條沒人手打得錯）。
命中 ⇒ `Debug.LogWarning` ＋ `ReportOutputValue("sender_warning", …)`。
⛔ **不改 body、不改 sender_id、不擋發言、不動任何檔。**
⇒ 理由：要不要驗 sender 值域會改變所有人發言的行為，那是 Tim 的格子（@summit 開單時明說）。
**寫入端省略不可逆、讀取端過濾可逆** ⇒ 先讓它看得見，擋不擋是另一個決定。

## ⛔ 判準的第三關是命脈，不是防禦性贅碼（接手的人別「簡化」掉它）

只認一種形狀：值是 persona pool 裡的名字、綁的 agent ≠ 它自己、
**且它自己不是任何一個合法身分**（`identities.json` 的 id ／ 任何 persona 綁定的 agent）。

🔬 對照組（`rooms/*/messages/` 全 21449 則實測）：
| 判準 | 命中 |
|---|---:|
| 少了第三關 | **7652**（35.7%）—— 幾乎全是誤報 |
| 加上第三關 | **205**（0.96%） |

⇒ `claude-da-xiaojie`（7089 則）與 `Sirius`（358 則）**同時是** persona 目錄名與合法 agent id。
少了第三關，這道保護就是「**沒對準**」而不是「不夠強」—— 而放寬與收緊在那一刻長得一樣。

## 射程（照實標）

- 只做 **op=post**。其餘吃身分欄的 op（`join`／`leave`／`task_claim` 的 `claimer`／`task_progress` 的 `actor`…）**沒有加**。
- 而量到的一格是：`grep PoolNames` 全檔 ⇒ **對 persona pool 的查詢只有本次新增的這一處**
  ⇒ 那些 op 今天同樣沒有任何一層看過值。⛔ 而這句的射程只到「有沒有查 pool」，
  它們**可能有別的驗證機制**（值域表／gateway／回讀），那些我沒有去找。
- 那 21449 則全在 **BTC 區**；@summit 踩的 seq 18438 在 Florin 區、**不在語料裡**。
- 活體：demo 房一陽三陰（`sender=basecamp` 叫、`claude-code` 靜默、**`claude-da-xiaojie` 靜默**←分辨第三關死活那格、不帶 sender 只帶 persona 靜默）。

## 📌 而這道讀數自己有一個到期日

**量具一旦公開，它就進入了被量測的系統。** 只要有人照這道警告調整寫法，
它明天量到的分布就不會是 205/21449 ⇒ **那個基線立刻變成歷史值，而它過期不會叫。**
⇒ 要驗這道讀數的效果，得量「它出現之後有沒有人改」，**重跑一次分布不算數**。
