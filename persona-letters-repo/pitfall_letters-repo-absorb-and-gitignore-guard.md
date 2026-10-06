---
id: pitfall_letters-repo-absorb-and-gitignore-guard
topic: persona-letters-repo
title: letters 原地 init 後掛 submodule 要 absorb；舊 .gitignore 要先驗私密格
type: pitfall
status: active
created_at: 2026-10-06
created_by: unknown
links: []
related_docs: []
---

**症狀**：先在信件庫原地 `git init`、再在父層 `submodule add` ⇒ git 走 Adding existing repo，`.git` 留在工作樹是**目錄**（其他 persona 是指向 `.git/modules/…` 的指標檔）；且舊 `.gitignore` 不檢查就 `git add -A`，私密檔可能進第一筆。

**可行動守則**
- `persona-create op=repo` 現在：init 前驗 `.gitignore` 有 `sealed/`、`/profile/_session.json`、`/cmd/*`（缺就擋、零寫入）；submodule add 後跑 `submodule absorbgitdirs`（父層已登記那條也補做）⇒ 舊現場重跑 op=repo 即修。
- 手修：先 `git -C <letters>/<p> fsmonitor--daemon stop`、關掉開著那個 repo 的 GUI（Fork），再 `git -C AgentCommands submodule absorbgitdirs ChatTavern/baton/letters/<p>`；讀回 `.git` 是指標檔、HEAD 與 log 不變、core.worktree 與他人同形。

**出處**：TASK-0432（erina 現場 2026-10-06），SCP_Core f01ab74、Senate fe998da。
