---
id: pitfall_trigger-missed-during-domain-reload
topic: reading-library-cmd
title: trigger 落在 domain reload 窗口被靜默漏接（RunCount=0 但 Editor 活著）
type: pitfall
status: active
created_at: 2026-08-06
created_by: summit
links: []
related_docs: []
---

## trigger 落在 domain reload 窗口 → 靜默漏接（2026-08-06 兩次血證）

**症狀**：`run_cmd.py` 等到 timeout，queue 內該筆 `RunCount: 0`（從未被執行）。
Editor **還活著**（`AgentCommands/PromptQueue/_treasury_state.json` 90 秒內仍被寫）。

**真因**：recompile → domain reload 會重載 FileSystemWatcher；在那個窗口寫進去的
`pending.trigger` 沒有人收，之後也不會補掃 —— 檔案還在，事件沒了。
兩次都是「我剛跑完 `run_cmd.py run Recompile`，緊接著送下一筆 cmd」。

**繞法**：重寫 `pending.trigger`（換 createdAt）補一次事件；或人工按 ▶ Run Pending。
**根治方向**：watcher 重新註冊後應**主動掃一次 queue 有沒有 pending 且 trigger 還在**
（不靠事件，靠對帳）。這是「不會叫的壞掉」家族：檔案在、cmd 在、Editor 在，只有事件不在。

## 附帶的 answered-alarm（訊息比事實大）

`run_cmd.py` timeout 時印：「Editor not running, or UCL_AgentCommandWatcher disabled?」
—— **兩個猜測今晚都不是真因**，而附了猜測的警示會讓人不去查真因（詞條 `answered-alarm`）。
建議改成先列可查證的事實：Editor 心跳時間戳 / 該 cmd_id 的 RunCount / trigger 檔是否還在。

## check_compile.py 的 --watch 語意鬆

`--watch` 曾在新編譯紀錄落地前就回傳，印出**改動前**那一筆的 timestamp 且**不帶 STALE 警示**
（不帶 --watch 直接查才會印 STALE）。所以：`--watch` 回來後仍要核對 timestamp 晚於改動時間。
