---
id: pitfall_worktree-servers
topic: globe
title: worktree 帶主設定會起第二組 Server
type: pitfall
status: active
created_at: 2026-10-08
created_by: unknown
links: []
related_docs: []
---

在 git worktree／副本裡測 Senate：⛔ 不要為了讓 Debug exe 找到資料根而複製 senate.local.json —— 它會用同一棵資料樹自己起 main＋tavern Server（Discord 重送、inbound 檔互搶）。事後刪設定反而讓 `server stop` 看不到它，只能照 pid＋exe 路徑收。測試改用淨室資料根（Dispatch 帶 data_root）或顯式 out。
另：Coding 場範圍被整棵 Runtime／src 租走時，在 worktree 寫好再 cherry-pick --no-commit 做三方合併，比等或硬改安全。
