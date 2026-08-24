---
id: decision_contract-task-memory
topic: task-management-system
title: 契約：Task ↔ 工作記憶（欄位／不互寫／不一致只印／錨點在 Task 檔）
type: decision
status: active
created_at: 2026-08-24
created_by: basecamp
links: []
related_docs: []
---

# 契約：Task ↔ 工作記憶（TASK-0015 C# 半 ✕ TASK-0017 python 半）

Tim 2026-08-24 指示：**會改到 C# 的部分交給 @summit**。⇒ 一條雙向連結被兩個人各實作一半，
所以欄位與格式**必須先談定**，否則兩邊各自定義＝漂移，而漂移的症狀是「單子指的記憶跟記憶指的單子不一樣」，
兩邊都不報錯。

## ① 欄位（兩邊各擁有自己那格，**不互相寫**）

| 側 | 欄位 | 型別 | 誰寫 |
|---|---|---|---|
| Task 檔 frontmatter | `memory_topic` | 字串（＝主題目錄名，單值） | Cmd_Task（C#，@summit） |
| Task 檔 frontmatter | `memory_archived_commit` | 字串（sha，空＝未歸檔） | Cmd_Task（C#，@summit） |
| 主題卡 `_topic.md` | `task_indices` | int 陣列 | work_memory.py（python，@basecamp） |
| 主題卡 `_topic.md` | `status` / `archived_at` / `archived_commit` | 字串 | work_memory.py（python，@basecamp） |

⛔ **不做互相寫入**：A 側不去改 B 側的檔。理由是這條連結有兩個獨立的寫入者
（Cmd 與 CLI，不同語言、不同 process），互寫就是分散式寫入衝突，而它會在併發時安靜地覆蓋。

## ② 不一致怎麼辦：**印出來，不自動修**

晚安對帳第三段同時印兩類：
- 單子有 `memory_topic` 而該主題的 `task_indices` 不含這張單 ⇒ 印「單向連結」
- 單子未關而主題的 `state` 超過 N 天沒更新 ⇒ 印「久未更新」（Tim 要的跨多日守門）

⇒ 沿用本專案既有判準：**寫入保存事件，讀取決定怎麼看**（meadow）。
自動修會讓「誰寫錯的」這件事永遠查不到。

## ③ 穩定回憶的錨點：**Task 檔**

Tim：「回看 task 時需要能穩定的回憶相關記憶」。
⇒ 錨點放 **Task 檔**，因為記憶會被歸檔或刪除，而**Task 檔一定還在**（它是承諾紀錄）。
- 主題還在 ⇒ `op=show` 印主題的最新 `state` 摘要
- 主題已歸檔 ⇒ 印「已歸檔（`archived_commit`）」
- 主題已刪除 ⇒ 印「已刪除，紀錄在 `memory_archived_commit`」
⛔ **三種情形都不准印成「沒有記憶」** —— 那三者同形就是「找不到 vs 什麼都沒有」那隻病。

## ④ 歸檔的前置守衛（血證驅動）

歸檔／刪除**前**機械檢查該主題目錄在 git 裡是乾淨的。
🩸 2026-08-24：我寫完四筆記憶後 `git status` 顯示 untracked ——
在 `8c77758` 之前，「刪掉也沒關係，git 有」是**假的**。
📌 **「反正 git 有」不是狀態，是一個需要被驗的前提。**

## ⑤ 誰做哪半

- **@summit（TASK-0015，C#）**：`memory_topic` / `memory_archived_commit` 欄位＋讀取端
  （`op=show` 印摘要、`op=list --arg memory_topic=` 可篩）、晚安對帳第三段。
- **@basecamp（TASK-0017，python）**：`work_memory.py` 的 `archive` 寫入端（含 ④ 的 git 守衛）、
  主題卡 `task_indices`、`read` 印關聯單現況、刪除時留墓碑。
