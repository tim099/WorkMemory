---
id: pitfall_two-book-stores-same-names
topic: reading-library-cmd
title: 「C# store」有兩個：按 op 名字判等價會判錯，按寫入目錄判才對
type: pitfall
status: active
created_at: 2026-09-07
created_by: basecamp
links: [reading-library-cmd/pitfall_booknotes-store-not-empty]
related_docs: [Assets/Plugins/SCP_Core/Runtime/Cmd/SCP_Cmd_Book.cs, task:TASK-0143]
---

**這個工作裡「C# store」有兩個，而它們住在不同目錄、名字近到會互相冒充。**

| store | 位置 | 誰寫 |
|---|---|---|
| 舊 store（書本筆記／authored 寫書線） | `BookNotes/<slug>/book.json` ＋ `chapters/` `characters/` `arcs/` | `library.py`（add-book/log-chapter/arc…）與 `SCP_Cmd_Book` |
| 新 store（work→media→reader） | `BookNotes/Library/work|media/…/reader.json` | `Cmd_Library op=media_init` 等 |

🩸 **踩法（2026-09-06 → 09-07，同一個人兩天兩個方向）**：
我 09-06 逐份點名「6 份活的 `book.json` 對 C# store 檔數」，量的是 **Library 那個 store**
⇒ 得出「@gura《深海對拍錄》與 @Sirius《熄燈前的燈》C# 那側 0 檔 ⇒ 孤兒」，
並據此把 TASK-0143 ②-bis 寫成硬閘、排了一個「含資料遷移」的解法 (a)。

09-07 重量：`senate cmd book --arg op=writing` **當場列出那兩本**（讀自 `BookNotes/<slug>/book.json`）
⇒ **它們從來不是孤兒**；C# authored 線的 store 就是舊 store。資料一個位元組都不必遷移。
真正缺的是**寫入端**：`log-chapter` / `arc` 在 C# 這側零實作（只有 `op=add`）。

⚠ 而最會冒充的一對是 **`library.py log-chapter` ↔ `Cmd_Library op=note_chapter`**：
名字幾乎同義，但**寫的是不同 store** ⇒ 「C# 已經有等價 op」那格如果只按名字判就會判錯，
而失效樣子是「兩邊都在動、各動各的目錄，沒有一層報錯」。

## 判準（下一個人照做就好）

1. 問「哪一個 store」之前**不要**回答任何「C# 有沒有」的問題 —— 那個查詢沒有定語。
2. 判「C# 有沒有等價 op」用**它寫到哪個目錄**判，⛔ 不用 op 名字判
   （通則已記在 `decision_aclass-splits-into-two-families`）。
3. 要知道 authored 那條線 C# 讀不讀得到 ⇒ 跑 `senate cmd book --arg op=writing`，
   它每一列都印**讀自哪個檔**（那行就是定語）。
