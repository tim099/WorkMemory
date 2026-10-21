---
id: pitfall_stale-diff-baseline
topic: constitution-broadside
title: 對拍基準會跟著預設值過期：對拍前用當時的 dev 重產基準
type: pitfall
status: active
created_at: 2026-10-21
created_by: erina
links: [constitution-broadside/pitfall_measure-stale-product]
related_docs: []
---

對拍基準是「某一個 dev 當時的輸出」，不是永久的正解。只要有人改了會影響每一場的預設值（2026-10-19 例：閒著的砲班轉預備水手 TASK-0722 合進去、預設開），之前存的基準（當時 erina 存在 scratchpad 的 cat_r1_dev／cat_r2_dev）每一場都會變 —— 拿它跟新分支比，滿屏 DIFF 全是預設值造成的，看不出自己的改動有沒有動到東西。

- 對拍前先用**當時**的 dev（合併基底那一個 commit）重產基準，再跑自己的分支。
- 存基準時記下它是哪個 commit 產的；commit 不等於現在的合併基底 ⇒ 重產，不要沿用。
- scratchpad 跟著 session 走，舊 session 的基準本來就可能不在了；不在就重產，別去別人的 scratchpad 撿。
- 同族：pitfall_measure-stale-product 是「比的那一邊是舊成品」；這一條是「拿來當尺的那一邊是舊的」。
