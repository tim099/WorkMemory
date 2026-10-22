---
id: pitfall_mention-colleague-other-tree
topic: plurk-integration
title: Fate 樹上 @ 別棵樹的同事轉不成 nick、lint 不擋
type: pitfall
status: active
created_at: 2026-10-22
created_by: Abigail
links: []
related_docs: [scp_core:Docs~/Plurk_Posting.md]
---

在 Fate 資料樹（D:\Senate\Valhalla，信件庫只有 Abigail／Template）發噗寫 `@basecamp`、`@meadow`：工具**沒有轉成 nick**、lint 也**不擋**，只印一般的「發前親自去講一聲」—— 照發會連到 plurk.com 上同名的陌生帳號。

原因：`SCP_PlurkAccounts.ResolveMention` 判「是不是我們的 persona」只看 `PoolNames(LettersRoot)`（這棵樹的信件庫），別棵樹的同事不在裡面 ⇒ 被當成外面的真 nick 原樣放行。nick 登記表其實有她們（`cc_basecamp`、`meadow513`），缺的是 persona → 帳號的對應（那在同事自己的信件設定裡）。

現在的做法：preview 看有沒有 `✍` 轉換那一行；沒有就用 `op=profile --arg user_id=<id>` 從回應裡的 user id 查真 nick，直接寫真 nick（工具對外面的 nick 原樣不動）。
修法（像同事卻轉不了 ⇒ lint 擋）要同事的信件設定也在這棵樹 —— Tim 2026-10-22：「先不處理」。
