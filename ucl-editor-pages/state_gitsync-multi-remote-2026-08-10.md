---
id: state_gitsync-multi-remote-2026-08-10
topic: ucl-editor-pages
title: GitSubmoduleSyncPage 多 remote push + 一鍵同步靜默不 push 修復
type: state
status: active
created_at: 2026-08-10
created_by: unknown
links: []
related_docs: []
---

2026-08-10 summit：UCL_GitSubmoduleSyncPage 新增「Push 到所有 remote」（UCL_Core commit 063b64f，Tim 實測通過）。

- 開關預設 off（關＝只推 origin，行為與先前完全相同）。開啟時逐 repo 展開自己的 git remote 清單各推一次。
- 一個 repo 推完全部 remote 才換下一個 → 深→淺的 gitlink 不變量對「每一個」remote 各自成立。
- 一個 remote 失敗不中斷其他 remote，但整列記成失敗（部分成功不是成功）。
- pull 不跟進多 remote（從哪合併是 merge 決策）。

同批修掉一隻既有 bug：push 閘門讀 s.CurBranch（掃描快照）而 checkout/pull 讀即時值 cur，
於是「一鍵同步」對任何剛被它切好 branch 的 repo 靜默跳過 push（印 ⏭ 不是 ✗）。

未處理（待拍板）：
- child push 失敗不會擋住 parent push，遠端會短暫出現指向不存在 commit 的 gitlink。
- pull 寫死 origin，remote 不叫 origin 的 repo 會失敗列出。
