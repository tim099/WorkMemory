---
id: state_analysis-2026-09-20
topic: chess-senate-migration
title: 現況分析與四階段方案（⛔ 未開單未動工，球在 Tim）
type: state
status: active
created_at: 2026-09-21
created_by: basecamp
links: []
related_docs: []
---

Tim 2026-09-20 問：chess.py 能不能廢棄、下棋流程全面遷 Senate CLI（**Codex 環境跑不了 python**）。以下是當天現量的讀數與我交的方案，⛔ 尚未開單、尚未動工。

## 現況（量的）

- `senate cmd` **沒有** chess；Editor **沒有** `Cmd_Chess` ⇒ **`chess.py` 是棋的唯一實作**。
  ⇒ 沒有半套要對齊，但 C# 那側一行都沒有 ⇒ **這是重寫，不是遷移**。
- 1178 行，**零第三方相依**（只用標準庫）⇒ 西洋棋規則是手寫的。
- 資料：`AgentCommands/Chess/games/*.json`（25 局）。

## 拆帳（成本差一個量級）

| 段 | 行數 | 搬它等於什麼 |
|---|---|---|
| 引擎 T01（FEN／合法走子／入堡／吃過路／升變／重複局面／子力不足） | **348** | 真正要新寫的，正確性敏感 |
| 流程（start/join/release/lobby/match/move/resign/draw/list） | 440 | 中等 |
| 存檔 T03 | 64 | 低（JSON schema 原樣沿用） |
| 渲染 T04 | 19 | 低 |
| 發券 T06 | 70 | **刪不是搬** —— `UCL_VoucherAuthority` 是現成對口 |
| 廣播 T05 | 73 | **刪不是搬** —— `Cmd_Tavern op=post` 是現成對口 |

⇒ 143 行直接蒸發。⭐ 而流程那段裡的 `pick_match_candidate` / `count_moves`
**已經在 `UCL_FreeTimeGating.cs` 有第二份 C# 實作**，該檔自己標著
「⛔⛔ 判準重複警告 —— 這裡與 chess.py 是同一份判斷的兩份實作」
⇒ 搬過去正好消掉這筆既有債。

## ⭐ 驗收：資料已經把黃金對照表準備好了

每一步 history 都存著 `prior_fen` + `uci` + `result_fen` ⇒ **25 局 320 步**。
新引擎的閘：逐步餵 `(prior_fen, uci)`，輸出 FEN 必須跟 `result_fen` **逐位元組相同**。
⇒ 這一格不需要任何人判斷、不需要我當證人（而我造的證人永遠同意我）。

⚠ **但 320 步是語料覆蓋率不是規則覆蓋率**：入堡／吃過路／升變／三次重複／50 步這幾條裡
語料沒走到的，對拍會**全綠**。⇒ 那幾格要另補手寫案例，
⛔ 不准拿 320/320 當「引擎對了」。

## ⚠ 風險：21 局正在進行中

`status` 分布：`in_progress` **21** ／ `resigned` 2 ／ `checkmate` 2。
⇒ 不能要求大家重開棋局 ⇒ **JSON schema 不准動**，新實作必須原地接手。
這也讓切換可逆：兩套實作共存一段時間，同一局兩邊都讀得懂。

🩸 而我第一次量這格是**錯的**：我用 `status != 'ACTIVE'` 判定，得到「25 局全部已結束」——
而實際值是 `in_progress`。那個錯答案長得完全合理，
⇒ **「我不知道那一格叫什麼」與「那一格是空的」在輸出上同形**，這正是這次遷移最大的風險形狀。

## 建議切法（四階段，每階段自己的紅燈）

1. C# 引擎 ＋ 320 步對拍閘（不過就不往下走）＋ 補手寫案例覆蓋語料沒走到的規則
2. 存檔／渲染／流程搬過去，**schema 不變** ⇒ 21 局原地接續
3. 券／廣播接現成 API（刪 143 行）；`UCL_FreeTimeGating` 那兩份重複實作收斂成一份
4. `chess.py` 退場（依專案慣例整支刪除，不留 stub 墓碑）

## ⚠ 這把尺量不到的（射程）

「Codex 跑不了 python」的射程**遠大於棋**：`Tools~/AgentCommands` 有 **36 支** python，
skill 指名的還有 `run_cmd.py`／`knowledge_base.py`／`spend_menu.py`／`sculpt.py` 等。
⇒ 若目標是「Codex 環境能完整工作」，應**先列一份 Codex 實際會撞到哪幾支的清單再排序**，
⛔ 不是從棋開始就一定對。📌 球在 Tim（等他決定優先序再開單）。
