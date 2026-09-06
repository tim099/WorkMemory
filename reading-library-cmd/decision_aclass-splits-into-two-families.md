---
id: decision_aclass-splits-into-two-families
topic: reading-library-cmd
title: A 類分裂成兩族：Cmd_Books 同 store 可對拍／Cmd_Library 不同 store 也不同鍵 ⇒ 那四支不能用「同名 op」退場
type: decision
status: active
created_at: 2026-09-06
created_by: basecamp
links: []
related_docs: []
---

## A 類分裂成兩族：`Cmd_Books` 同 store（可對拍）／`Cmd_Library` **不同 store 也不同鍵**（不可退場）

**實測 2026-09-06（basecamp，TASK-0143）。這一條推翻盤點表 A 類的一半。**

盤點表把 9 支標成「C# 已有同名 op ⇒ python 退場」。⚠ 它自己寫著那是**名字層**。
補完行為層之後，A 類**不是一族，是兩族**，而分界線不在名字，在**它們寫到哪個 store**。

### 族一：`Cmd_Books`（`tips`／`donations`／`donate`／`publish`／`tip`／`shelf`）—— **同一個 store**

兩邊讀寫同一份 `AgentCommands/BookNotes/`（打賞簿 `Books/tips/`、捐贈 `_donation.json`）。
⇒ **對拍有意義**，而且今天兩支通過（資料相同、呈現不同）並已退場。

### 族二：`Cmd_Library`（`note_chapter`／`bookmark`／`add_character`／`revise_view`）—— **不同 store、不同鍵**

| | python（`library.py`） | C#（`Cmd_Library`） |
|---|---|---|
| 落點 | **舊 store** `BookNotes/<book>/chapters/`、`characters/` | **新 store** `BookNotes/Library/media/<media_id>/readers/<persona>/…` |
| 鍵 | `--book`（書 slug） | `mediaId` ＋ **`persona`** |
| 章號 | `ch01_<slug>.md`（**兩位數**，由 `int` 格式化） | `chapters/0003/`（**四位數 id**，`0000` 保留給序章） |
| 內容模型 | 固定四段模板（內容摘要／關鍵事件／人物新認識／伏筆） | 自由 `body` ＋ **round 版本史**（`r<N>_<date>.md` ＋ `chapter.json`） |
| 附帶 | 回寫 `book.json` 的 `progress` | 心得同步發酒館、把 seq 寫回該 round 當 receipt |

**磁碟讀數（⛔ 不是讀參數說明推的）**：
- python 形狀 `ch[0-9][0-9]_*.md`：全 store **2 個檔**（都在 `basecamp-foot-of-the-mountain`，⚠ 我自己的書）。
- C# 形狀 `chapter.json`：**305 個**。

逐鍵對照（三支寫入型，取 IO 層真實簽章，不是 ArgsSchema 字面）：
- `bookmark`：py `--book --chapter --note` ／ C# `(mediaId, persona, note, impression, status)`
  ⇒ **C# 這支根本沒有 chapter**，多了 `impression`／`status`。
- `add-character`：py `--book --id --name --name-original --chapter --headline --facts`
  ／ C# `(mediaId, persona, characterId, name, nameOriginal, facts, view)` ⇒ py 的 `--headline`／`--chapter` 沒有對應。
- `revise-view`：py 多 `--diff`／`--headline`／`--chapter`，C# 沒有。

### 判定

**族二那四支不能用「C# 已有同名 op」當退場理由。** 它們不是同一支的兩個實作，
是**不同資料模型上的相似操作** —— 名字近而已，跟 `add-book` ↔ `media_init` 同一族的錯。

⛔ 而且更硬的一格：族二的 python 端寫的正是**舊 store** ⇒ 它們跟 `add-book` 一樣
**落在 TASK-0143 ②-bis 的管轄範圍**（舊 store 去留 2026-09-06 拍 (b)：現在不搬、python 不退場）。

### 動作型判準（第二次踩到同一個形狀了，寫成通則）

**盤點表上「C# 有同名 op」這一格，在量到它們寫到哪個目錄之前，一律當「未分類」。**
今天兩次都是這個形狀（早上 `add-book` ↔ `media_init`、下午 `log-chapter` ↔ `note_chapter`）——
⇒ 兩邊都能跑、都不報錯，**分得開它們的不是名字也不是參數表，是它們各自寫到哪一個目錄**。

### 順帶一格（Q0，已在別處記）

`Cmd_Library.cs` 檔頭仍寫著「**目前寫入本體未實作 —— 規格待 Tim 拍板**」，
而它現在真的會呼叫 `UCL_ReadingLibraryIO.*` 寫檔（305 個 `chapter.json` 是證據）。
⚠ 那句話**過期了**，而我差點拿它當「C# 那側是空的」的證據 —— 檔頭註解也會 stale。
