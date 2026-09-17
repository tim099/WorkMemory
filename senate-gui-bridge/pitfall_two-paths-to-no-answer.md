---
id: pitfall_two-paths-to-no-answer
topic: senate-gui-bridge
title: 「問不到窗」有兩條路（心跳 4 秒 / 逾時 10 秒），只修一條不會叫
type: pitfall
status: active
created_at: 2026-09-17
created_by: summit
links: []
related_docs: [task:TASK-0229, task:TASK-0233, commit:07547a8]
---

## 兩條「問不到窗」的路，門檻不同 —— 改措辭時要同時改

`senate ui --list`（與所有走 GuiBridge 問窗的指令）有**兩條**會走到「沒有答案」的路，
而它們的觸發門檻差很遠：

| 路 | 觸發 | 落在哪 |
|---|---|---|
| **心跳過期**（常走） | `GuiBridgeStatus.Alive` ＝ 心跳年齡 ≤ `GuiBridge.HeartbeatStaleSeconds` = **4.0 秒** | `GuiBridge.PrintNotRunning` |
| **送出後逾時**（少走） | Probe 說活著 ⇒ `Send` 等 `RequestTimeoutMs` = **10 秒** 無回應 | `Program.cs` 的 `aRes == null` 分支 |

⇒ **窗停筆超過 4 秒就走不到第二條**。人隔一會兒才去問，走的一定是第一條。

🩸 TASK-0229 只改了第二條（因為交來的重現讀數是 `taskkill` 完**立刻**問，剛好落在 4 秒內），
於是「窗卡住（⇒ 等）」與「窗已關掉（⇒ 重開）」這兩個**處置相反**的成因，
在第一條路上原封不動併成一句「窗可能卡住或已被關掉」，而那支手上就有 pid、沒去問作業系統。
TASK-0233 把三種成因的措辭收成 `GuiBridge.PrintWhyNoAnswer(pid, symptom, err)`，兩條路共用。

⛔ 下一個人要注意的兩格：
1. **要改這類措辭，先問「這句話有幾個地方會印」** —— 兩邊各留一份，下次只會有一邊被改，而沒被改的那邊不會叫。
2. **`ProcessAlive` 的三種回傳都要處置**：`false`＝查無此行程（重開）／`true`＝行程還在（等）／
   `null`＝問不到（⛔ 不猜）。而 `true` 與 `null` 兩支**至今沒有實跑樣本** ——
   前者要能凍住行程的工具（本機沒有 pssuspend），後者要偽造心跳檔（⛔ 不做：偽造的讀數跟真的長得一樣）。
