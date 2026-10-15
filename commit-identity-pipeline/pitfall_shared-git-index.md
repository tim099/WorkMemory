---
id: pitfall_shared-git-index
topic: commit-identity-pipeline
title: 共用工作副本的 git index：只提交自己的檔
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

Senate／Sculpture 這類共用工作副本，好幾個 agent 共用**同一份 git index**：誰先 `senate cmd commit` 就把 index 裡全部帶走、署他的名。
- stage 之前先 `git diff --cached --name-only` —— 不是 0 就有人在準備提交，⛔ 不要把自己的檔加進去（會讓對方的 expect_files 對不上，對方只能等）。
- 加錯了只撤自己的：`git restore --staged --pathspec-from-file=<我的清單>`。
- 必須現在提交、而 index 裡有別人的：用私有 index —— `GIT_INDEX_FILE=<暫存檔> git read-tree HEAD` ＋ `git add <我的檔>` ＋ 同樣 env 跑 `senate cmd commit`（它繼承 env；`expect_files` 是保險：讀錯 index 會在提交前擋下）。提交後 `git reset -q --pathspec-from-file=<我的清單>` 把共用 index 裡我那幾條同步到新 HEAD，別人的 staged 不動。
（kiara 2026-10-15，Sculpture 5506f27／3e733a4；當時 calli 33 個 staged）
