---
id: state_progress-2026-08-07-day-end
topic: reading-library-cmd
title: 進度快照：人物 op 與 brief 產檔完成，剩發文整合與管理頁
type: state
status: superseded
created_at: 2026-08-07
created_by: summit
links: [reading-library-cmd/state_progress-2026-08-06-movie-pause, reading-library-cmd/state_progress-2026-08-07-sirius-converged]
related_docs: []
---

## 進度快照（2026-08-07 00:10 收工，取代 movie-pause 那份）

### 今晚新增並實跑（在「電影前停手」那份之後全部做完）
- **人物 op 上線**：`add_character` / `revise_view`（Tim 要求別拖到明天，原話「越累積越多了」）
  - 不覆寫是硬規則：重複 add_character 被 reject 並指路 revise_view（反向測試通過）
  - revise_view 版本號**掃磁碟取 max+1**，永不覆寫；`change_reason` 必填
  - `view` 必填 —— 只記 facts 不記看法，這套系統就退化成人物百科
- **recall 產 brief 檔**：`WriteRecallBrief()` → `letters/<persona>/_reading_recall_<media-id>.md`
  **沿用 Python 版同一路徑與檔名**（不另開位置）；frontmatter 對齊 `_wake_brief.md` 慣例
- **RenderRecall 補人物段**：profile facts ＋ **所有 vN 看法版本並列**（不只最新版）；
  順帶修掉「沒有章節就提早 return」會吞掉人物段的 bug
- `note_chapter` / `bookmark` 後自動重生成 brief（stale 投影比沒有投影更糟）
- 修掉 UCL_ReadingLibraryIO.cs 被 grep 判為 binary：我寫「JSON 逃脫」註解時把 \u0000-\u001F
  寫成字面，真的把控制字元寫進檔案 —— **在講逃脫的註解裡犯了逃脫問題**

### 全鏈已實跑（陪看兩場當測資）
media_init → note_chapter ×2 → add_character ×5 → bookmark → recall(產檔)。
反向守衛三項各驗一次：media_kind 與前綴不符 / 缺 persona / 重複 add_character。編譯 errors 0。

### 未做（優先序）
1. **發文整合**：`Cmd_Tavern` 開 internal post 回 seq → `RecordSharedSeq` 落 `shared_seq`。
   **不可自己呼叫 `WriteMessageWithSeq`**（會漏 mirror / inbox 路由 / mention / 計酬）。
2. **`UCL_ReadingNotesManagePage` 接 `RenderRecall`**（Tim 要 QA 那個頁面的讀取）。
3. **Python `library.py reading-recall` 退位** —— 否則就是我反對過的兩套實作。
4. `op=scan` / `op=migrate`（Archive 候選掃描 + 人工裁決遷移）。
5. queue 失敗不堵塞 —— **必須與 run_cmd 判定端成對改**（見 decision_queue-failure-must-not-block）。
6. `revise_view` 正向路徑未實跑：不為測試編造假改觀，第一次真改觀時才會走到。

### 已提交（2026-08-07 00:0x，9 筆，全部落追蹤分支、薪資 9/9 已領）
UCL_Core `548b62d` + `f5d2bda`／BookNotes `77c14bb` + `e3b7427`／WorkMemory `f3a6b19`／
letters-summit `71a02c9`／AgentCommands `05859b7e` + `[chat] 6b822f76`／主專案 `d4db5f0`。
收工前 Tim 另授權 commit all + push（其他人都睡了），該筆之後另計。
