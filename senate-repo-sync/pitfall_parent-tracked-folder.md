---
id: pitfall_parent-tracked-folder
topic: senate-repo-sync
title: 外層已追蹤的資料夾不算巢狀 repo 的邊界（skip-worktree）
type: pitfall
status: active
created_at: 2026-10-22
created_by: Abigail
links: []
related_docs: [senate:Docs/Workflows/Repo_Sync.md]
---

外層 repo 已經追蹤了某個資料夾裡的檔，那個資料夾就**不算**巢狀 repo 的邊界（2026-10-22 實驗）：在裡面 `git init` 之後，改過的檔照樣在外層列成 ` M`、新檔照樣列成 `??`，外層一跑 `add -A` 就整份收進去 —— 兩個 repo 記同一批檔。

Fate 資料樹的 WorkMemory/ 就是這樣（10-21 複製進來的兩個主題被外層追蹤）。

做法（SenateRepoSyncGit）：未追蹤的 ⇒ 外層 `info/exclude`；已追蹤的 ⇒ 外層 `update-index --skip-worktree`。兩者都是本機、不提交。⛔ 不替人 `git rm --cached`：那是資料樹的一筆提交，會從每一份 clone 的工作區刪掉那些檔 —— 由人決定（這台的 6 個檔還等 Tim）。

旁支坑：`git check-ignore` 對巢狀 repo 會讀到它**自己的** .gitignore，回錯的「已忽略」（CRLF＋空行的那份就會）⇒ 判「排除了沒」看外層 `status` 有沒有 `??`，不看 check-ignore。
