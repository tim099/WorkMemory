---
id: state_survey-20260825
topic: timezone-unification
title: 三套曆現場普查與遷移狀態（2026-08-25 快照）
type: state
status: active
created_at: 2026-08-25
created_by: basecamp
links: []
related_docs: [repo:docs/Glossary/utc-everywhere-local-display.md]
---

# 三套曆的現場普查（2026-08-25 快照）

⚠ **這是快照不是規格。** 規則本身在 glossary 詞條 `utc-everywhere-local-display`，
本檔只記**現場長什麼樣、遷移到哪**。遷移完成後本主題歸檔。

⚠⚠ **這份清單是 grep 到的，不是「全部」。** 口徑不是世界的邊界 ——
2026-08-25 這桌一天內撞了四次（ERE 的 `\|`／`grep -c` 判行尾／`split(":")` 在 Windows／
「零 `Task.Run`」的射程只掃了兩個檔）。要當成「已知清單」用，不要當成「完整清單」用。

## 拍板

- **Tim 2026-08-04**：日期一律走 UTC，統一時區。
  🩸 出處只有 `UCL_BartenderDaemon.cs:947` 一則註解 —— 文件端**零命中**（見 TASK-0046）。
- **Tim 2026-08-25**：全系統一律 UTC，**只有顯示轉當地時間**。
- **Tim 2026-08-25（收工閘專項）**：不能用日期判斷，要用「本次醒來期間有動哪些」。

## C# 端：目前仍用本地時間做「日期」的位置

| 位置 | 用途 | 備註 |
|---|---|---|
| `UCL_BartenderMentionService.cs:117` | 酒保 mention 回覆上限 `replied_today` 的跨日歸零 | 本地日 ⇒ 半夜 12 點重置 |
| `UCL_BooksIO.cs:533` `Today()` | 書籍相關的「今天」 | 本地日 |
| `UCL_ReadingLibraryIO.cs:539` `Today()` | 閱讀庫的「今天」 | 本地日 |
| `Cmd_StreamWatch.cs:2306` | `DateTime.Today.Add(...)` 組當日時刻 | ⚠ 這格可能是**正確的本地用法**（它在算「今天幾點」給人看的時段），需個別判 |

⇒ 對照組（已是 UTC）：`UCL_AwakeningService.cs:1295`、`UCL_BartenderDaemon.cs:954`、
`Cmd_GoodMorning.cs:191`（酒館訊息分夾）。

## python 端：`datetime.now()` 未帶時區的位置

- `library.py:125`（`Today()`）、`library.py:1841`
- `sculpt.py`：280 / 284 / 337 / 501 / 669 / 670 / 799 / 858（多為 `created_at` / `timestamp`）
- `mbti.py:229`、`run_cmd.py:607`（後者是 cmd_id 的時戳，**檔名用，不是判定用**）

⚠ 這裡要分兩類，**不可一起改**：
1. **判定／比對用的時間** ⇒ 該轉 UTC
2. **檔名／顯示用的時間戳** ⇒ 轉了會改變檔名格式，動它要另外評估相容性

## 已經「根本不用曆」的（最佳解，不是遷移目標而是範本）

- 收工閘（`UCL_TaskReconcile.cs:221-227`，`ea33cbf`）：比對 `locked_at`（本次上線）
  ⇒ **零日曆、零時區轉換**。它不是「符合全系統 UTC」，是**根本沒有曆可以錯**。

## 遷移狀態

- [x] 拍板寫成可 grep 的東西（glossary 詞條，TASK-0046）
- [ ] `UCL_BartenderDaemon.cs:947` 註解加一行指向詞條（⏳ 等 C# 空出來，不搶 summit 的工）
- [ ] 上面 C# 四格逐格判：哪些該轉 UTC、哪些本來就該是本地（顯示層）
- [ ] python 端兩類分開處理
- [ ] ⚠ **再掃一次找沒被列到的** —— 本清單是已知清單不是完整清單
