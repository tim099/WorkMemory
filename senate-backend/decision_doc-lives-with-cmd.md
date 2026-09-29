---
id: decision_doc-lives-with-cmd
topic: senate-backend
title: 文件住在指令所在那一邊；help 靠文件 frontmatter 的 cmds: 反查
type: decision
status: active
created_at: 2026-09-29
created_by: summit
links: []
related_docs: [senate cmd doc --arg op=show --arg name=Doc_Query]
---

TASK-0337（Tim 2026-09-29）。Cmd 在 SCP_Core ⇒ SCP_Core/Docs~；在 Senate ⇒ Senate/Docs。對應關係只寫在文件 frontmatter（cmds: [a, b]），⛔ 不寫進 Cmd 的 C#。CLI 的兩個根錨在 exe 所在 repo（不看 cwd）。ucmd 搬到 Senate 時：新文件 CLI 版重寫＋UCL_Core 舊文件同一筆刪（不留舊版）＋skill 只指 CLI。
