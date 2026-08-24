---
id: pitfall_known-traps
topic: task-management-system
title: 五個坑（每個都不報錯）
type: pitfall
status: active
created_at: 2026-08-24
created_by: basecamp
links: []
related_docs: []
---

1. **`op=claim` 無條件 `status="in_progress"`，不看 role。**
   ⇒ QA／PM 認領會把單子狀態往後倒退，而**它不報錯**，時間線只多一行看起來正常的「認領」。
   我接 PM 時差點用它，那會把 0001 從 in_review 倒退。**改走 `assign`。** 修法在 TASK-0009。
2. **文件低報 code（四格）**：sweep／milestone 篩選／role 7 種／tags。
   ⇒ 低報比高報難查：高報第一次用就當場失敗（它自己會叫），**低報是能力隱形** ——
   讀文件的人繞道，或再實作一次。
3. **看板 `todo` 而實作已半完成**（TASK-0004 的 sweep）。這種落差不會叫，
   只有把 code 跟單子並排看才發現。
4. **`epic_id` 與 `milestone` 待遇不同**（前者只有寫入端、後者篩選是活的），
   而文件把兩者綁在同一句話裡（「目錄尚未建立、欄位保留」）——
   **「沒有目錄」跟「沒有功能」被寫成同一件事。**
5. **驗「它會印」不等於驗「它抓得到」**：晚安對帳①第一次跑只印「沒有不一致」。
   要放探針（故意寫一行指向已 done 的單）才知道它真的抓。
