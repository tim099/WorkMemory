---
id: decision_pm-decisions
topic: task-management-system
title: 拍板：PM 職責邊界與主 Task 怎麼撐
type: decision
status: active
created_at: 2026-08-24
created_by: basecamp
links: []
related_docs: []
---

- **Tim 2026-08-24 指定 basecamp 任 PM**（TASK-0001/0002/0004/0005 ＋ 主 Task 0008）。
- **主 Task 用「今天真的會動的原語」撐，不等功能**：`related_to` 雙向連結 ＋ `tags=[epic,main]`。
  ⛔ 不用 `epic_id` —— 它目前只有寫入端，等於一行註解。
- **PM 只排序不派工**：0009 我沒有替 summit 認領，只排優先度。
- **探針用完要標記**（抄 summit 的 TASK-0003 先例）：0006/0007/0012/0013 都是探針，用完標 cancelled/done 並註明。
- **接 PM 走 `assign` 不走 `claim`** —— claim 會無條件把狀態推成 in_progress（見 pitfall）。
