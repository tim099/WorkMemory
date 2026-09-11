---
id: decision_chess-match-and-session-free-2026-09-11
topic: freetime-cmd-flow
title: 下棋四格拍板（自動配對／不綁場次／只在輪到自己置頂／看得見有人在等）—— 而四格裡兩格是零改動
type: decision
status: active
created_at: 2026-09-11
created_by: calli
links: [reading-library-cmd/pitfall_bytewise-diff-not-axis-list]
related_docs: [task:TASK-0206, task:TASK-0207, commit:8e9b6a44, commit:122410fd, commit:5f41ab9d]
---

Tim 2026-09-11 一天內拍了四格，落在 `chess.py` ＋ `UCL_FreeTimeGating` ＋ `Cmd_FreeTimeActivity`。

## 拍板內容與落點

| 拍板 | 落點 | commit |
|---|---|---|
| 自動配對：有可加入的局就入座，沒有就開一局**自己跟自己下** | `chess.py match` | `UCL_Core 8e9b6a44`（TASK-0206） |
| 候選多筆時挑**已走手數最少**（同手數取小 index，可複驗） | `pick_match_candidate` | 同上 |
| **下棋不綁自由時間** | 活動 md 宣告式 `needs_session`（預設 true） | `UCL_Core 122410fd`（TASK-0207） |
| Chess 優先層**只在輪到自己時**觸發；很久沒下走通用飢餓那條 | `UCL_FreeTimeGating` case Chess | `2b246d28` |
| 骰面看得見「**有人開了一局在等**」 | `TryFindJoinableChess` | `5f41ab9d` |

⭐ 兩格「本來以為要做、量完發現已經有了」：
1. **「開局＝自己跟自己下」** —— `start --side` 的預設值本來就是 `both`。
2. **「很久沒下過棋也觸發」** —— 飢餓置頂是**通用**的，住 `Cmd_FreeTime`、判準不看活動是什麼，
   而 switch 裡根本沒有 `case Default` ⇒「參考 Default」就是走那條。
⇒ 判準：**拍板之前先量「這條是不是已經在了」** —— 那天四格裡有兩格是零改動。

## ⛔ 兩個接手一定要知道的邊界

### ① 判準重複：C# 骰面 ↔ python 配對（躲不掉，已互指）
`TryFindJoinableChess`（C#，決定要不要置頂）與 `chess.py pick_match_candidate`（python，真的去配）
是**同一份判斷的兩份實作**，跨語言共用不了。
⇒ **漂掉的症狀是「骰面說有一局在等，而 `match` 去了卻開了新局」** —— 兩邊都不報錯。
兩邊都留了指回對方的註解。改任一邊的過濾條件（status／OPEN 座／solo／排除自己在座）或排序鍵
**必須同時改另一邊**。

### ② `needs_session: false` 時 `activity` 無處可 fallback
沒有場次 ⇒ `iSession` 是 null ⇒ `activity` 只能靠呼叫端顯式帶。
沒帶就照舊擋（並印出「帶上 activity 可能就過」那句出口）。⛔ 不猜活動 —— 猜錯會把下棋記成閱讀。
⭐ 而場次計數器與飢餓統計**切得開**：`RecordPick(iPersona, id)` 只吃 persona
⇒ 無場次時 `activities_done` 不動、飢餓統計照記。兩件事都在回傳檔明講。

## 🩸 而最值得記的讀數不是 code，是它上線十分鐘後發生的事

早上量到：最後一次開新局是 **2026-08-13**（29 天前）、`--vs-open` 躺三個月、
徵人廣播全酒館 **5 筆、最後一筆 08-17**。
`5f41ab9d` 上線之後十分鐘：@kiara 在她骰面上看到
`🪪 @calli 開了一局在等（第 8 局…；共 2 局在等）` ⇒ **接了還走了一步**；@Sirius 接了另一局。

⇒ **積木一直都在（`--vs-open` / `lobby` / `join` 三個月前就全在），缺的只是有人替它喊一聲。**
📌 一般形：**做一個功能之前先問「它會出現在誰的必經路徑上」** ——
不在任何人必經路徑上的功能，等於沒做。
