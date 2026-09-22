---
id: pitfall_two-worktrees-two-binaries
topic: bank-demurrage-voucher
title: 改的那份與驗的那份不是同一份 code（多工作副本 ＝ 多 binary）
type: pitfall
status: active
created_at: 2026-09-23
created_by: kaguya
links: []
related_docs: []
---

**同一個 repo 的多份工作副本 ＝ 多份 binary。**

`SCP_Core` 同時掛在 `Bar/Assets/Plugins/SCP_Core`（Unity 端）與 `Senate/SCP_Core`（`senate.exe` 由它建置）。
2026-09-22：我在 Unity 那份改完、`unity-recompile` 綠、`ucmd run Invoke` 驗過 ——
而 `senate cmd demurrage-voucher` 那條路**量到的是另一份 code**（當時停在 `c5ee766`）。
⇒ 它印出來的舊行為**格式完整、數字合理**，沒有任何一層會說「你驗的不是你改的」。

**判準**：改完 C# 要驗之前先問「我要跑的那支 binary 是哪一份 code 建的？」
⇒ 走 Unity 端 `ucmd run Invoke`（同一份 code 剛編譯過）才量得到 Unity 副本的改動；
要 CLI 那條路量得到，得 `git -C <Senate/SCP_Core> pull --ff-only` ＋ `./build.sh` ＋ `senate server stop/start`。

⚠ 另一格：**常駐 Server 有版本守衛**（`delegate_failure = build_mismatch`）——
它擋得對，⛔ 別把它讀成「Cmd 壞了」。
⚠ 而 `senate server start` 是前景常駐：從 agent 背景跑，呼叫結束它就跟著死
（實測：背景 task 結束後 `server_state = not_running`）。委派型 Cmd 會自己拉起一顆（TASK-0267 實測成立）。
