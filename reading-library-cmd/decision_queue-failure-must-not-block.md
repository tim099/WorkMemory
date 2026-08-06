---
id: decision_queue-failure-must-not-block
topic: reading-library-cmd
title: 失敗的 Cmd 直接移除不堵塞 —— 但必須與 run_cmd 判定端成對改，否則失敗長得像成功
type: decision
status: active
created_at: 2026-08-06
created_by: summit
links: []
related_docs: []
---

## 失敗的 Cmd 不該堵住整個 queue（Tim 2026-08-06 拍板）

**現況**：OneShot 失敗後**留在 queue**（設計上是為了讓 `LastRunError` 看得見），
而 `run_cmd.py` 送新批次前會檢查「上一批是否還 pending」→ **一筆失敗就把後續全部擋住**，
必須人工進 Editor 清。今晚實撞：反向測試失敗後，我後續每一筆都被
「Previous batch still 'pending' after 60s」擋掉。

**Tim 的方案**：把失敗的 log 等輸出出來（`.py` 端也要有通知），並**直接從 queue 移除、不堵塞**。

**⚠ 必須成對改，否則生出比堵塞更糟的東西**：
`run_cmd.py` 現在是用「cmd 從 queue 消失」推論成功（印 `✓ Cmd disappeared from queue → Success`）。
**失敗後直接移除 → 失敗會長得跟成功一模一樣**（不會叫的壞掉 / 量了一個替身）。

### 實作清單
1. **Editor 端（`UCL_AgentCommandRunner`）**：OneShot 失敗 →
   ① 寫 `_cmd_errors/<cmd_id>.md`（已有）
   ② **寫一筆結果進 `queues/<agent>/History`**（成功與失敗都寫，含 cmd_id / 時間 / 例外型別 / 訊息）
   ③ 從 `queue.json` 移除 → 不堵塞後續
2. **`run_cmd.py` 端**：改掉「消失＝成功」——以 **cmd_id 查 History／錯誤報告**判定；
   查不到才報 unknown（不可預設成功）。並在失敗時把錯誤訊息**直接印在 stdout**，不只給檔案路徑。
3. 順手修 timeout 訊息的 answered-alarm（見 pitfall fragment）。

**影響面**：框架層，全部 36 支 cmd 共用 → 改完要跑一輪既有 cmd 的正反向煙霧測試
（至少 Tavern post / Recompile / 一支會失敗的），不能只測 Library。
**時機**：Tim 指示看電影後再收（不在他要用工具的前一刻改工具底座）。
