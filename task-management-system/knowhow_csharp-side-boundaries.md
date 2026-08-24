---
id: knowhow_csharp-side-boundaries
topic: task-management-system
title: C# 那半的四塊落地與各自沒驗到的那一格（summit）
type: knowhow
status: active
created_at: 2026-08-24
created_by: summit
links: []
related_docs: []
---

C# 那半今天蓋完的四塊，以及**每一塊的邊界在哪**（給接手的人，不是給我自己）。

## 落地（都有實跑讀數，SHA 在 Task 的 commit_shas）

| 塊 | 內容 | 沒驗到的那一格 |
|---|---|---|
| P0 框架 | Models/IO/Cmd（一單一檔、index 自我修復、blocker/QA 兩道閘） | 無（五格都撞過） |
| P1 閉環 | `Fixes/Refs TASK-n` → `op=commit`（狀態機只有這一份） | 公告失敗 exit 6 時單子不動：**只有 code 讀數**（basecamp 簽「由結構保證」不簽「已驗」） |
| P3 後台頁 | 列表／篩選（PopupSearch）／留言區（展開才顯示） | 留言區展開後的版位**沒有第二次眼睛驗** |
| 記憶錨點 ＋ 收工 | `memory_topic` 四種答案／晚安 ④a④b／`op=wrapup`／收工閘 | **跨夜**：閘的判準是「今天動過」（UTC 日期），午夜前後語意模糊 |

## 判準（這些比 code 更難重建）

1. **回讀要印「這次動過的那一格」，不是印一組固定欄位。**
   🩸 `op=link` 的回讀原本只印 blocked_by/blocks/related_to，而 `subtask_of` 改的是
   `epic_id`/`subtask_indices` ⇒ 那行對父子關係什麼都證明不了，卻長得跟證明過一樣。
2. **「已歸檔」「連結壞了」「沒有記憶」「沒掛」四種答案不可以同形。**
   前兩者若都印成「沒有記憶」，就是「找不到 vs 什麼都沒有」那隻。
3. **可跳過但留名，比不可跳過更持久。** 硬擋會讓人在真的沒東西可寫時去找繞過的方法，
   而繞過一次那道閘就永久失效。⇒ 跳過理由寫進**那張單的時間線**，不是寫進 log。
4. **警示不是擋**（自動結單那格）：擋會讓真正不需要 QA 的小單無法自動結，而那是設計要的。
5. **同一個量就該一個常數**（`STALE_DAYS` 同時服務 sweep 與晚安 ④b）；不同的量才需要各自的常數。

## 契約（跟 python 側的接縫，不可單方面改）

- Task 側擁有 `memory_topic` / `memory_archived_commit`；記憶側擁有 `task_indices` / `status`。
  **兩邊不互寫** —— 兩個獨立寫入者（不同語言、不同 process），互寫＝分散式寫入衝突，會安靜覆蓋。
- C# 要寫記憶 ⇒ **代跑 `work_memory.py`**（`UCL_TaskWorkMemoryCli`），走 `ArgumentList` 不拼字串，
  內文一律 `--body-file`。
- 路徑走既有解析器 `UCL_EditorPath.CorePath`。🩸 我第一版寫了不存在的 `UCL_CorePath.CoreRoot`。

## 明天第一件

`TASK-0017` 我是 QA：**實際造一筆 untracked 記憶再跑 archive**，確認 git 守衛擋得住
「沒入版控就刪」。⚠ 不採信「我實跑過」的敘述 —— 那一格的失敗不可逆。
