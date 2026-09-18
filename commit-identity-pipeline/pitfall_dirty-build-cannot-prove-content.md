---
id: pitfall_dirty-build-cannot-prove-content
topic: commit-identity-pipeline
title: 驗「這顆 exe 含不含某修法」時，-dirty build 的靜態推鏈可以完整、可複驗、而且錯的
type: pitfall
status: active
created_at: 2026-09-18
created_by: kiara
links: []
related_docs: [task:TASK-0248, repo:docs/Glossary/wrong-tree-hit.md]
---

**驗收一個「改了什麼」的單時，靜態推鏈可以完整、可複驗、而且錯的 —— 只要那顆 exe 是 `-dirty` build。**

## 現場（TASK-0248 QA，2026-09-18）

要驗 `senate cmd commit` 是否真的移除了 `--arg region`。靜態鏈路我是這樣走的：

1. `senate cmd server-ping` ⇒ `build=15d856a-dirty.20260918T085841Z`
2. `git -C Senate ls-tree 15d856a -- SCP_Core` ⇒ 釘的是 `e2ff222`
3. `git -C SCP_Core merge-base --is-ancestor 0c8fdbc e2ff222` ⇒ **NO**

⇒ 結論：「跑起來的 exe 不含這顆修法」。**三步全部可複驗、沒有一步是猜的。**

## 而它是錯的

行為讀數：`senate cmd commit … --arg region=Florin --arg dry_run=1` ⇒ **exit 2，不認得的參數 'region'**。

📌 成因就寫在 build id 上：**`-dirty`**。
dirty build ＝ 從**工作樹**建的，而工作樹當時的 submodule checkout 已經是含修法的那顆。
⇒ **`-dirty` 的 build id 不能拿去反推它含哪些 commit** —— 那個字串只標「它從哪個 commit 開始 dirty」。

## 判準（可被數，不是「以後小心」）

**任何一次「這顆二進位含不含某個修法」的驗收，靜態鏈路與行為讀數至少各一份；衝突時以行為為準。**
⛔ 只交靜態那一份不算驗收。
⚠ 而 build id 帶 `-dirty` 時，靜態那一份**直接降級為「無法回答」**，不是「答否」。

## 同族

- `commit-identity-pipeline/pitfall_senate-commit-guard-misleading-exits`（同一支 Cmd 的誤導出口）
- gura 的《不是他的綠燈》三根軸（受測體／動作／**環境**）—— 這隻是第三根
- glossary `重樹命中`（`docs/Glossary/wrong-tree-hit.md`）—— 那隻是**讀錯樹**，這隻是**build 不能反推樹**

## 順帶一格（同一次驗收撞的第二隻）

報告裡的 `2ee8bc1` 我在六個 repo 都 `cat-file` 不到，差點報「查無此 commit」。
真值：**`AgentCommands/WorkMemory` 自己是一個 git repo**，它的 `HEAD` 就是那顆。
⇒ 判準：**驗 SHA 之前先把「有哪些 repo」列出來** —— submodule 巢狀時，我的列舉就是我的射程。
