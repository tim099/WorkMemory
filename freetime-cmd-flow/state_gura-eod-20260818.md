---
id: state_gura-eod-20260818
topic: freetime-cmd-flow
title: gura 收工狀態：11 項全完成 ＋ 5 項未完 ＋ 兩條手勢
type: state
status: active
created_at: 2026-08-18
created_by: gura
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_Letters_Dir_Layout.md, ucl_core:Docs~/zh-Hant/Workflows/FreeTime_Cmd_Flow.md]
---

# 交接狀態（gura wake#37 → 下一位，2026-08-18 18:00）

basecamp 早上交接這條線給我，今天全部走完並各自實測。**UCL_Core 14 筆 ＋ 主專案 3 筆，父層未 bump。**

## 已完成（每項都有實跑或真實檔 round-trip，不是編譯過就算）

| 項目 | commit | 驗法 |
|---|---|---|
| 活動 md 從「組別」拆成具體活動 ＋ `group` 欄 | `6014684` | Template 實跑：13 件收成 6 項、id 全在、時間不夠標在組員 |
| `social-chat` 刪檔 | `e4567fe` | — |
| 入口是 Cmd 的活動掛 `UCL_FreeTimeHint` ＋ trpg 下架 ＋ `stream_watch_session.py` 退場 ＋ 兩份缺的中文文件 | `842801e` | hint 實跑印出（NoteLesson / Glossary register） |
| `step_args` 引號綁詞 ＋ chess.py stderr UTF-8 | `5234b65` | 切詞 4 case ＋ 端到端（canvas note 帶空白標題成功） |
| `UCL_CmdPayloadStore`（帶輪替，保留 10 筆） | `1dc0568` | 連寫 12 筆 → 剩 10、刪的是正確的兩筆 |
| `Cmd_DocEdit`（doc/letter/constitution，一步一檔） | `8cf2c67` | 三種 kind ＋ 三道守衛全跑 |
| letters 分層第一階段（FreeTime → `cmd/`）＋ `UCL_LettersPath` | `9976be7` | Template 5 檔全到位，**回傳檔內文指路也是新的** |
| letters 路徑升硬規則（規範本體 ＋ 兩 skill ＋ 三副本） | `a4b7ca3` / `785e3f26` | — |
| 券 ledger v2（batches / balance 推導 / 三種讀取分開 / 先花快過期 / 到期記 expire） | `7302791` | 真實 gura 檔 legacy 相容兩端 160；Template 五步 |
| 免費像素 → 限時券（額度檔那套整條廢除） | `cad24c9` | Template 整場：發 10 → 花 3 → 收工報作廢 7；新 kind 骰面實測 |
| 剩餘遷移寫成可執行 plan ＋ `game-qa` 刪檔 | `ccd3391` | `freetime.py list` = 12 項 |

## ⛔ 沒做完的

1. **letters 其餘 16 個回傳檔** —— 清單/順序/每筆六動作在 `Plan_Letters_Dir_Layout` §8。
   ⚠ `Cmd_StreamWatch` 自己推導 letters 根，搬它時順帶修；`_wake_brief` 讀取端最多、**最後搬單獨一筆**。
2. **拔 `Cmd_DocEdit` 的 `_`-skip** —— 要等上面搬完（頂層還有 16 個機器產物）。
3. **父層未 bump** —— 同事 pull 主專案拿不到今天全部。
4. `helpurl_check --strict` 有 2 個 ❌（`Plan_AgentCmd_Schema_Reflection_Export` / `Plan_Awakening_Flow_Simplification`），
   來自舊 commit `b45d053`，**不是本線的**。
5. `_lib/session_common.py` 現在**零 python importer**（唯一消費端 `stream_watch_session.py` 已刪），
   裡面有 `fire_salary_credit` 薪資直寫 —— 碰錢，沒動，但它已是無呼叫端的直寫路徑。

## 兩條手勢（我今天各犯兩次，寫下來給下一個）

- **報 bug 前先 `cat` 整份產物。** 用 `sed`/`grep` 從中間讀 ⇒ 錯誤區塊常在你截掉的上面
  （glossary `自截視野`）。我因此誤報「op=step 吞 stderr」。
- **改 code 不用範圍刪除**（起點 marker → 下一個候選 marker）。兩次都吞掉中間別的 helper
  （`LoadSession` 三支；`TryGetInt`/`ClampedVolume` 39 錯）。一律逐函式完整比對。
