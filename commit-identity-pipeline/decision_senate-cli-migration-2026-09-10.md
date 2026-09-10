---
id: decision_senate-cli-migration-2026-09-10
topic: commit-identity-pipeline
title: 提交入口搬進 senate cmd commit；vendors/models 寫死，但 agent_emails 的分裂沒治
type: decision
status: active
created_at: 2026-09-10
created_by: gura
links: []
related_docs: []
---

Tim 2026-09-10 拍板：`git_commit.py` 整支搬進 `senate cmd commit`（SCP_Core），舊入口退成 exit 2 指路 stub（不刪），`agent_email.py` / `agent_model.py` / `hooks/commit-msg-validate.py` / `install_hooks.py` **整批刪除**。

三個刻意不同（不是漏搬）：
1. 多位參與者 `personas=a,b,c`（`SCP_CmdArgs.Get` 是單值介面）
2. **公告失敗拆成 exit 6（確定沒發，補發安全）與 exit 7（不知道，先回讀）** —— python 併成一個 6 並無條件叫人補發，而它自己的註解就寫著「同一個 SHA 貼兩次＝付兩次錢」。⇒ 紀律寫在 code 裡卻沒寫進出口。
3. 不收 `--strict-email-source`：C# 側直讀 `profile/`、沒有快照那一段 ⇒ 條件永遠不成立。**收一個永遠不生效的旗標比不收更糟（它看起來像一道防線）。**

vendors／models 兩張表**寫死進 `SCP_AgentModelRegistry`**（不留檔案覆寫）。理由是那個檔是專案級而 UCL_Core 掛在多棵樹底下 ⇒ 同一位同事從不同的樹提交會得到不同 trailer（實測：`Zeta@summit(Claude / claude-opus-5)` 與 `zeta@summit(claude-opus-5)` 同一天並存）。留「可覆寫」的入口＝把分裂原封不動留著。

⚠ **未解**：`agent_emails.json` 同族但**沒治好** —— 三棵樹三份，LY 與 Bar 的 `Codex`／`ClaudeCode` **對調**。沒有自己 `profile/email.md` 的人（meadow／Sirius）信箱會因樹而異，而它進 `Co-Authored-By` 就改不掉。Tim 已澄清 email 可公開 ⇒「defaults 也寫死」是可行選項，等拍板。
