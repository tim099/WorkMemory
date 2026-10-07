---
id: pitfall_delete-feature-family-checklist
topic: unity-senate-migration
title: 刪一整族 Unity 功能：先 grep 引用分類、刪完重編、殘留 using 不會當場報錯
type: pitfall
status: active
created_at: 2026-10-07
created_by: unknown
links: []
related_docs: []
---

刪一整族 Unity 功能（TASK-0458／0459）的做法與會咬人的地方：

1. 刪之前，對每個要刪的型別在刪除範圍外 grep（`-w`，連 `.asmdef`／`.json`／`.asset` 一起），把引用分成「真的用」「範例字串」「註解」「using」四類。
2. 刪完一定重編。殘留的 `using` 指向已刪的命名空間時，刪檔當下不會報錯，只在下一次編譯才叫（這次是 `UCL_ControlPanelPage` 的 `using ...AgentCommands.ChatTavern`，內文根本沒人用）。
3. 反向對照要挑「留下來、而且與被刪功能無關」的指令實跑（`ucmd run TypeInspect op=inspect`、`unity-recompile`），ucmd 參數少給時失敗是參數問題，不是刪檔造成；先看錯誤報告再下結論。
4. 另一個宿主的編譯錯誤會讓 Unity 不載入新組件：Editor 裡查到的是舊程式。要驗 runtime 行為（例：`UCL_URL.HasResolver("scp_core")`）得等整個專案的 error 清掉。
5. 主目錄 build 被別人未提交的改動弄紅時，用 `git worktree`（含 SCP_Core 子模組的 worktree）在乾淨 HEAD 上 build 自己的檔，不去動別人的檔。
