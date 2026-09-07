---
id: pitfall_cleanroom-pointer-beats-env
topic: reading-library-cmd
title: library.py 的 clean-room：pointer 快照贏過 CLAUDE_PROJECT_DIR（會靜默寫到真資料根）
type: pitfall
status: active
created_at: 2026-09-07
created_by: basecamp
links: []
related_docs: [Assets/Plugins/UCL_Core/Tools~/AgentCommands/_lib/ucl_paths.py, task:TASK-0143]
---

**要把 `library.py` 指到暫存資料根做 clean-room：`CLAUDE_PROJECT_DIR` 不夠 —— pointer 快照贏過它。**

`_lib/ucl_paths.py` 的 `data_root()` 是 tier-0 先讀 `read_pointer()`（Editor 寫下的
`<UCL_Core>/.agentcommands_root.local`），**tier-1 才是 `CLAUDE_PROJECT_DIR`**。
⇒ 只設 env 就跑，會靜默寫到**真資料根**。

🩸 血證（2026-09-07 basecamp，TASK-0143 ②-bis 對拍）：我設了 `CLAUDE_PROJECT_DIR` 就跑
`add-book`，`cr-test-book` 被建在**真的** `BookNotes/`（已 `rm -rf` 清掉並回讀：
`git status` 空、活的 `book.json` 回到 8 份）。⚠ 失效樣子是**指令成功、輸出正常**——
它印出的落點路徑就是真根，而我當時只看「✅ 建立」那一行。

## 可行做法（實測會生效）

1. 把 `<UCL_Core>/Tools~` **複製**到 `<tmp>/UCL_Core/Tools~`（`_find_ucl_core_dir` 是往上找
   名為 `UCL_Core` 的 ancestor ⇒ 複製品自己就是一個 core）。
2. 在 `<tmp>/UCL_Core/.agentcommands_root.local` 寫 `repo_root=` 與 `data_root=`（絕對路徑）。
3. 跑**複製品**裡的 `library.py`。
4. ⭐ **陽性對照不可省**：跑完立刻確認真資料根**沒有**那個測試 slug
   —— 「寫進暫存根」與「寫進真根」在 stdout 上長得幾乎一樣。

⛔ 不要改共用的那份 `.agentcommands_root.local`（Editor 在寫它，那是別人的狀態）。
