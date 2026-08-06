---
id: state_progress-2026-08-06-movie-pause
topic: reading-library-cmd
title: 進度快照：三 op 已實測 / CJK 修正未驗 / 發文整合與管理頁未做
type: state
status: active
created_at: 2026-08-06
created_by: summit
links: []
related_docs: []
---

## 進度快照（2026-08-06 22:10，電影前停手）

### 已實測（有證據）
- `Cmd_Library` op=`paths` — 路徑解析，寫 `_last_op.md`（原本只 Debug.Log → 呼叫端讀不到，已修）
- op=`media_init` — 建 work.json / media.json / reader.json / bookshelf.md 四檔；已存在不覆寫
- **反向**：`media_kind=series` 配 `film-` 前綴被擋（Tim 截圖確認錯誤訊息）
- **反向**：缺 `persona` 被擋（「必填且無預設值」）
- op=`note_chapter` — r1／r2 同章並存不覆寫；`display_number: "Part 1"` 人話；`time_range` 落章節層
- body 含反引號與 `$` 走 `--arg-stdin` 不被 shell 咬

### 已寫但未實跑驗證 ⚠
- **CJK unescape**：`JsonData.ToJsonBeautify()` 把中文寫成 `\uXXXX`（既有 Python 端寫的檔是原生 UTF-8）→ 已加 `UnescapeNonAscii`，**編譯過但沒跑過一次寫入**。
  卡住原因：note_chapter 0002 那筆 trigger 落在 domain reload 窗口被漏接，queue 內 RunCount=0。
  驗法：跑 0002 那筆，看 `chapter.json` / `reader.json` 的中文是原生字元。

### 未做
1. **發文整合**（Tim 的原始需求核心）：`Cmd_Tavern.Op_Post` 是 private instance method。
   計畫：加 `internal int LastPostedSeq`（在 line ~804 `AppendMessage` 回傳處設）+
   `internal static async UniTask<int> PostInternalAsync(args, token)` → `Cmd_Library` 呼叫後
   `UCL_ReadingLibraryIO.RecordSharedSeq(...)` 落 receipt。**不可自己呼叫 WriteMessageWithSeq**（會漏 mirror/inbox/mention/計酬）。
   已有：`RecordSharedSeq` / `Key_SharedSeq` 已在服務層備好。
2. **管理頁讀取**：`RenderRecall(mediaId, persona, fullRounds, out error)` 已備，頁面接上即可（Tim QA）。
3. **Python 讀回退位**：`library.py reading-recall` 改成呼叫 Cmd 或退役（否則就是我反對的兩套實作）。
4. **`op=scan` / `op=migrate`**：Archive 候選掃描與人工裁決遷移（Q3/Q4/Q5）。

### 待清理
- 測試資料 `Library/media/film-selftest-zzz/` 與 `Library/works/selftest-zzz/` **要刪**（我的自我驗收產物）。
- queue 內 `20260806-220631-8a65bb-library`（note_chapter 0002）未跑。
- `commands_schema.json` 過期：`Library` 不在已註冊清單（每次呼叫噴降級警告）→ 跑 `ExportCmdSchema`。
