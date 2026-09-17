---
id: knowhow_registry-write-path
topic: library-media-migration
title: registry 的唯一寫入端與三格邊界（不回填 / 不擋建檔 / 兩支讀取端）
type: knowhow
status: active
created_at: 2026-09-17
created_by: summit
links: [senate-gui-bridge]
related_docs: [task:TASK-0171, commit:b681524]
---

## 動它之前要知道的四格

**① 寫入端只有一個：`SCP_LibraryScan.AppendRecord`**（append-only、依 `source_id` 判重、寫回用
2 空格＋冒號後空格貼齊手寫原樣）。`RecordBornNew` 是它的呼叫者，掛在 `SCP_LibraryInit.MediaInit`
**只有 media.json 真的被建立那一次**。⇒ 重跑 `media_init` 零寫入（實測 records 6→6）。

**② 記不進去不擋建檔，但一定印一行 ⚠**。media.json 已落地時帳沒記是帳的問題；
靜默漏記正是這個主題在修的病。

**③ ⛔ 既有 media 不回填。** 機械掃一遍會把**真的遷移過的**（arakawa）也標成 `born_new` ——
那是在帳上造假。只人工補了 `series-qi-pa-xiao-guo` 一筆（它有 TASK-0171 開單時落下的證據：
StreamWatch 場次 `sw-20260818T233343Z-summit`）。
⇒ **所以「registry 認得的 media」≠「Library 裡所有 media」**，射程要照實講。

**④ 讀取端分兩支，別混用**：`LoadMigratedArchiveSlugs`（只回 `migrated`，用途是**隱藏**）
是 `LoadArchiveDecisions`（回所有裁決 slug→state/reason）的投影。要問「這筆有沒有被裁決過」
用後者；要問「該不該隱藏」用前者。同一個 slug 有多筆時**取最後一筆**（append-only 的帳，後面那筆是新的決定）。

⚠ 沒有實跑樣本的一格：`AppendRecord` 在「registry 缺 `records` 陣列」時會拒絕並回 error —— 那要造壞檔才測得到，目前只讀過程式碼。
