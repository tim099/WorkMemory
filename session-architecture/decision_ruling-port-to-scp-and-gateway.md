---
id: decision_ruling-port-to-scp-and-gateway
topic: session-architecture
title: 拍板：Session 搬 SCP_Core／頁面不保留／結算不搬走 gateway（含 TrySettle→TryClose 的語意修正）
type: decision
status: active
created_at: 2026-09-04
created_by: basecamp
links: []
related_docs: []
---

Tim 2026-09-04 拍板：Session 層搬進 SCP_Core／Senate CLI，**Unity 端管理頁不保留**；
搬不動的那一格（觀影最小結算）不搬，由 Senate 側委派回 Editor。

## 為什麼結算不搬（這是約束不是偏好）

`Cmd_StreamWatch.SettleAsync`（`:2070`）內含 `UCL_TreasuryLedger.Credit`（`:2151`）
⇒ **結算就是金流**，而金流搬家是 **TASK-0106**，Tim 已拍 **B：記單不動**。
⇒ 任何「把結算一起搬過去」的方案都會踩到一塊已經拍過「不動」的東西。

## 採 (丙)：Cmd 原生在 Senate，只有「關場那一步」走 gateway

判準是 Tim 同日另一句：**「未來傾向整體遷移到 Senate ⇒ 很多是過渡期方案」**
⇒ 設計判準因此不是「最省事的過渡」，是 **「拆得掉的過渡」**：

| 方案 | 過渡件散在哪 | 0106 那天要拆什麼 |
|---|---|---|
| 整顆鈕走委派 | 頁面（pending 態／Editor 沒開態／非同步） | 拆 UI 狀態機 ⇒ 白寫 |
| **(丙) 只有關場走 gateway** | **一個 class**（`SenateSessionCloseGateway`） | 換掉它，介面／Cmd／頁面一行不動 |

樣板不是發明的：`SCP_ICanvasGateway` ＋ `SenateCanvasGateway` ＋ `SCP_CanvasGatewayHost.Factory`
（Tim 在 TASK-0114 拍過的「內部串 ucmd、不移植 ledger」，這是同一個判準的第三次）。

## 🩸 gateway 的語意在同一天改過一次 —— 而理由是我自己寫的碼推翻了前提

第一版是 `TrySettle`（只結算），前提「權威狀態先落地、再結算」。
而 Editor 的 `SettleResidueAsync` **靠 `active=true` 判斷**
⇒ Senate 先關場再委派結算的話，對面會回「這場已經收過工 ⇒ 未重複結算」——
**結算永遠不會發生，而兩邊都不報錯。**
⇒ 改成 `TryClose`（整步關場，權威狀態由那一端寫）。
⭐ 連帶好處比修 bug 本身大：**委派方不自己寫 session 檔 ⇒ 寫入端仍然只有一個**（TASK-0100 的主題）。

📌 判準留給下一個要加 gateway 的人：**先問「對面判斷『該不該做』時看的是哪個欄位」** ——
如果那個欄位正好是你打算先改的那個，你的呼叫順序就會讓對面靜默跳過。

## 過渡件的紀律（本單開始執行）

過渡件要在 code 標退場條件（`⏳ 過渡（退場條件：<X> 之後）`）並寫進單子驗收。
理由不是原則：手上已經有三個沒有退場條件的過渡件 —— `wake_brief.py` 備援、
UCL／SCP 兩套安裝器並存、`run_cmd.py` 遷移（拖到 TASK-0107 還開著）。
**沒有退場條件的過渡，會安靜地變成永久架構 —— 而那時它已經有人依賴了。**
