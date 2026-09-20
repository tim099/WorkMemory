---
id: decision_batch2-scope-10-to-6
topic: tavern-read-layer-csharp
title: 第二批射程 10→6：task_* 三支會寫（AutoRecoverStaleLeases → AppendEvent）
type: decision
status: active
created_at: 2026-09-21
created_by: summit
links: []
related_docs: []
---

TASK-0247 原本列 10 支純讀 op，實際交付 **6 支**（SCP_Core `c294d02`，`senate cmd tavern-read`）。

## 為什麼縮

`task_list` / `task_next` / `task_state` 都呼叫 `UCL_ChatTavernQuestIO.AutoRecoverStaleLeases`，
而那支（`UCL_ChatTavernQuestIO.cs:350`，36 行）**呼叫 `AppendEvent`** ⇒ 它落檔。

⇒ 它們是「讀為主、寫一格」，與 `catchup` / `inbox_read` 同一類，歸 TASK-0106 那一側。

⚠ **失效樣子**：平常沒有過期租約時它們**表現得像純讀** ——
所以「把它們當純讀」這個錯誤會在剛好有一張單過期的那天才第一次出事，
而那天沒有人在改這段 code。

## 交付的 6 支與它們真正碰到的 IO

`read`（GetRoom/Range/Search/Since/Tail）／`members`（LoadIdentities/LoadMembers）／
`listrooms`（LoadRooms/ReadCurrentSeq）／`events_since`（QuestIO.LoadAllEvents）／
`note_read`（GetNotePath/ReadNote）／`note_list`（GetNotePath/GetRoom/ListNoteKeys）
—— 被呼叫的 16 個方法逐一驗過，除了 `AutoRecoverStaleLeases` 全部純讀。

## ⛔ 與 Editor 端唯一的刻意差異：房間存在性

Editor 側只有 `Op_Read` 與 `Op_NoteList` 會擋；`members` / `note_read` / `events_since`
**打錯房名回空清單、exit 0** ⇒ 「這房沒人」與「沒有這一房」同形。本側一律擋。
⇒ 所以逐筆對拍**只涵蓋房間存在的情況**。
