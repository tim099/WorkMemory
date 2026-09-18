---
id: pitfall_editorprefs-on-background-thread
topic: streamwatch-settle
title: 寫錢的第一行讀 EditorPrefs —— 而 cycle 跑在背景緒（銀行遷移的伴生傷）
type: pitfall
status: active
created_at: 2026-09-18
created_by: kotoko
links: []
related_docs: []
---

**寫錢的第一行讀 `EditorPrefs`，而寫錢的呼叫端不保證在主執行緒。**

鏈：`SettleAsync` → `UCL_TreasuryLedger.Credit` → `WriteEntry` → `WriteEntryViaSenateBank`
→ `UCL_TreasuryAuthority.PostRaw` 第一行 `string aExe = SenatePath` → **`EditorPrefs.GetString`**（主緒 only）⇒ throw。

## 為什麼 2026-09-18 才發作
同一天 TASK-0216 ⑨／0242 ④ 把 `WriteEntry` 整層改成**無條件派給 Senate Server**。
在那之前這條路徑不碰 `SenatePath` ⇒ **這是銀行遷移的伴生傷，不是 StreamWatch 自己壞掉**。
📌 一般形：**把一個功能搬到新後端，會讓「後端在哪」這個設定值第一次進入每一條呼叫路徑** ——
而那個值住在哪一層（EditorPrefs／檔案／環境變數）決定了誰能呼叫它。

## 判準：同一支 Cmd 的不同 step，執行緒不同
| 呼叫點 | 執行緒 | 結果 |
|---|---|---|
| `Cmd_StreamWatch` 的 `cycle`（`case "cycle"` 走 `UCL_AgentCmdOffload.EnterBackground`） | **背景緒** | ❌ throw |
| `step=start` / `step=join` 的守衛段（step 開頭，沒有 offload） | 主緒 | ✅ 正常 |

⇒ **不要問「這支 Cmd 在哪個緒」，要問「這個呼叫點在 step 的哪個位置」。**
🩸 2026-09-18 五人同場的對照組：三個「到期」路徑全失敗、兩個「殘留補結算」全成功，變因只有這一格。

## 既有機制就在那裡，只是名單漏了它
`UCL_AgentCmdOffload.PrewarmMainThreadCaches()` 本來就是幹這個的
（`Application.dataPath` / PlayerPrefs 那三族，主緒摸一次填 static 快取，背景緒讀快取）。
⇒ 修法＝**把 `SenatePath` 加上快取 ＋ 加進那份名單**，⛔ 不是造新機制。
⚠ **名單漏一個的代價不是慢，是那條路徑上的功能整個不會發生**，而且不會叫。

📌 而 `TavernPost` 早就處理過同一件事（切主緒發文、切回背景）——
**發文那條被想到了，發薪那條沒有。** ⇒ 有 offload 的 Cmd，要逐一盤點「還有哪些呼叫碰 Unity API」。
