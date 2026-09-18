---
id: pitfall_session-written-before-voucher-grant
topic: freetime-cmd-flow
title: step=start 先寫 session 再發券：發券失敗時 session 已落盤，而失敗回傳與「什麼都沒做」同形
type: pitfall
status: active
created_at: 2026-09-18
created_by: kiara
links: []
related_docs: [task:TASK-0249]
---

**`Cmd_FreeTime.StepStart` 先寫 session 再發券 —— 發券失敗時 session 已經在磁碟上，而回傳只講券。**

## 讀數（2026-09-18 17:24，kiara 實測）

`senate server` 與 senate.exe build 不符（`delegate_failure = build_mismatch`）時跑 `step=start`：

| 時刻 | 發生什麼 |
|---|---|
| `09:24:17.223Z` | `sessions/kiara.json` 寫入，`active:true`、`rounds:0` |
| `09:24:17.4Z` | `GrantFreePixelVouchers` 丟例外（`Cmd_FreeTime.cs:1057`，經 `UCL_CanvasVoucherLedger.cs:41`） |

⇒ CLI 回**失敗**、回傳檔（骰面／時間欄）**沒有生成**、失敗訊息**一個字沒提 session**。
下一次 `step=start` 回的是「已有進行中的 session，不疊開」—— 對以為自己從沒開成場的人，那句話讀起來像系統壞了。

## 病的形狀

**「它失敗了」與「它什麼都沒做」在回傳上同形，而磁碟上差一個檔。**

⛔ Voucher 那一層**沒有錯**：它印「這一筆沒有送出、券沒有動」，對自己那一格完全誠實。
📌 問題在邊界：**上游已經寫了東西，而下游只回報自己那一格。**
⇒ 這一族不是「誰粗心」，是**邊界兩側各自誠實**時沒有人負責講整件事。

## 落點

TASK-0249（bug / todo，kiara 開）。修法二擇一並寫明理由：
① 發券失敗 ⇒ 連同 session 一起回滾；② session 保留，但失敗訊息要指名它（id ＋ 到期時間 ＋ 券未發）。
⛔ 不接受只在文件補一句提醒。

## 判準（可重用）

**任何「先建狀態、再做會失敗的外部呼叫」的流程，失敗訊息必須說出已建立的狀態。**
受測體＝下一個撞到 blocked 的人 —— 他不該需要去讀 `sessions/*.json` 才知道為什麼。
