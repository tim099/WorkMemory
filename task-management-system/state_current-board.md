---
id: state_current-board
topic: task-management-system
title: 看板快照與關鍵路徑（接手先讀這格）
type: state
status: active
created_at: 2026-08-24
created_by: basecamp
links: []
related_docs: []
---

## 現在誰在哪一格（2026-08-24 17:58 UTC+8 快照）

總 14 張／未關 7／被阻塞 1／stale 0。**關鍵路徑：TASK-0001 → TASK-0002**，
而 0001 卡在 QA 對話不是實作 ⇒ 0002 可以動。

| 單 | 狀態 | 誰 | 接回時要知道的 |
|---|---|---|---|
| 0008 | todo | basecamp(pm) | **主 Task（傘）**。`related_to=[1,2,4,5,9]`、`tags=[epic,main]` |
| 0001 | in_review | summit(dev) / basecamp(qa,pm) | QA 意見已寫進時間線：①②實跑過、③簽「由結構保證」不是「已驗」 |
| 0002 | in_progress | summit(dev) / basecamp(qa,pm) | 🛑 被 0001 阻塞。交付物 `UCL_TaskManagerPage.cs` 已存在 |
| 0005 | todo | gura(design) / basecamp(qa,pm) | 文件對帳輸入已餵給她；skill 已改（7 role + sweep），workflow:145 與 §3 標題待確認 |
| 0009 | todo | basecamp(pm) | **沒人認領**。父子關係一等公民（epic_id 讀取端／subtask 寫入端／tag 可篩／claim 不改狀態） |
| 0004 / 0011 | done | — | 都由 basecamp QA 驗完結單 |

⇒ **明天第一件**：0009 沒人接，而它是 Tim 要的「主 Task 追蹤」的地基。
