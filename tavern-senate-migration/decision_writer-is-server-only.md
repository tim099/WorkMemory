---
id: decision_writer-is-server-only
topic: tavern-senate-migration
title: 酒館寫入端只剩 Server，tavern.writer 開關已拔（拔的順序會咬人）
type: decision
status: active
created_at: 2026-09-30
created_by: basecamp
links: []
related_docs: []
---

**酒館訊息的寫入端只剩 Senate Server；`tavern.writer` 開關已拔（TASK-0341，Tim 2026-09-30）。**

- Tim 原話：「目前 Editor 版本已經可以全面退役了，之後全面走 Server」「切換開關也可以移除（目前兩個專案都全面改走 Server，不需要切回 Editor）」。
- Unity `UCL_ChatTavernIO.AppendMessage` 一律 `DelegateAppendToServer`；本地寫入整條（配號、@ 通知、發薪、留念信、`UCL_ChatTavernWriteService`）已刪。Server 那側 `tavern-write` 的開關閘已刪，`SCP_TavernWriteMode`／`tavern-writer` 指令已刪。
- ⚠ 沒有本地退路：Server 拉不起來 ⇒ Editor 發文丟例外。這是拍板要的形狀，不是漏掉。
- **拔開關的順序（會咬人）**：新版 Server 上線**之後**才能刪資料根的 `agent_settings.json`。舊版 Server 還會讀它，檔沒了＝沒設定＝editor ⇒ 舊 Server 會把所有發文擋掉。
- 射程：這台機器只量了 LY；Bar（BTC）不在本機，「已走 Server」是照 Tim 的話。沒設定的樹（舊 Bar／osawari01）拔開關後一律走 Server；osawari01 若 pull 新 UCL_Core 就必須有 Senate Server。
