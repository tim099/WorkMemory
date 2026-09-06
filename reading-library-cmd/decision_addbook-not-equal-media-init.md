---
id: decision_addbook-not-equal-media-init
topic: reading-library-cmd
title: add-book ≠ Cmd_Library.media_init（不同 store）⇒ 仍是 B 類，且被 ②-bis 阻塞不在本輪移植
type: decision
status: active
created_at: 2026-09-06
created_by: basecamp
links: [pitfall_booknotes-store-not-empty, state_inventory-20260906-library-py-35-subcmds]
related_docs: []
---

## `add-book` 對拍結果：**不等價於 `Cmd_Library.media_init`** —— 它仍是 B 類，而且被 ②-bis 擋住

**實測 2026-09-06（basecamp，TASK-0143 ①盤點表的「待對拍」那一格）**

盤點表原本標：「`add-book`（8 引用）⚠ 疑似與 `Cmd_Library.media_init` 重疊 ⇒ 待對拍，
**可能其實屬於 A 類**」。⇒ 對拍完了，**它不屬於 A 類**。

| | `library.py add-book` | `Cmd_Library.media_init` |
|---|---|---|
| 落點 | **舊 store**：`BookNotes/<slug>/book.json` ＋ `chapters/`／`characters/` | **新 store**：`BookNotes/Library/work|media/…` |
| 必填 | `title`（id 可由 slugify 推） | `persona` / `media_id` / `work_id` / `media_kind` / `title` |
| 校驗 | 無（書已存在就拒絕覆寫） | `media_kind` 白名單 ＋ **`media_id` 前綴須與 `media_kind` 同字**（兩欄互為校驗） |
| 語意 | 建一本書 | 建 work/media **＋ 把 persona 登記成這部的 reader** |
| **`origin=authored`（寫書線）** | ✅ 有：`author_persona` / `publish_status=draft` / `status=writing` | ❌ **沒有對應概念** |

⇒ **兩者是不同 store 上的不同東西，名字近而已。**

## 而它為什麼現在動不了

`add-book --origin authored` 寫的正是舊 store，而舊 store 裡有 **2 本活的、正在寫的書**
（@gura《深海對拍錄》09-01、@Sirius《熄燈前的燈》09-04，C# store 零檔案）——
那就是 TASK-0143 條文 **②-bis 的硬閘**。

⛔ 移植 `add-book` ＝ 把「往舊 store 寫」這件事再實作一次，
而舊 store 的命運（(a) 先搬資料再退 python ／ (b) authored 線留到那兩本發布）**還沒拍板**，
且那不是 dev 能自己挑的。

⇒ **判定：`add-book` 留在 B 類、狀態改為「被 ②-bis 阻塞」，不在本輪移植。**

## 動作型判準（給下一個接的人）

**盤點表上「疑似重疊」的格子，對拍前不准當成已分類。**
名字近的兩支很可能住在不同的 store 上，而**兩邊都能跑、都不報錯** ——
分得開它們的不是名字也不是參數表，是**它們各自寫到哪一個目錄**。
