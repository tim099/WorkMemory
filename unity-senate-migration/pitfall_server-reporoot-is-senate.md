---
id: pitfall_server-reporoot-is-senate
topic: unity-senate-migration
title: Server 傳給常駐工作的 repo 根是 Senate 自己的 repo
type: pitfall
status: active
created_at: 2026-09-28
created_by: gura
links: []
related_docs: []
---

**酒館 Server 傳給常駐工作的 `iRepoRoot` 是 Senate 自己的 repo（`D:/Unity/Senate`），不是資料所在的專案（`D:/Unity/Bar`）。**

- 症狀（2026-09-28 TASK-0323 實測 seq 22520）：Inbound 附件落地成功，但 refs 寫成**絕對路徑** —— `MakeRepoRelative` 比不到前綴、原樣回，而那個路徑照樣開得了圖 ⇒ 看起來沒壞。
- 酒館 refs 的慣例是「擁有這棵 AgentCommands 的專案」的相對路徑（畫布分享圖：`AgentCommands/Canvas/previews/...`）。
- ⇒ 要 repo 根就**從資料根往上推一層**（`SCP_DiscordMedia.RepoRootOf(iDataRoot, "")`；`tavern-write` 推 @ 通知用的也是這個），⛔ 不採用 `ServerHost` 傳進來的那一格。
- 🩸 淨室測試原本傳的是**對的**根，所以抓不到；抓到它的是 Tim 從 Discord 發的一張真圖。修好後測試改成故意傳錯的根（`D:/wrong-host-repo`）。
- 射程：搬任何「會寫 refs／會算 repo 相對路徑」的東西進 Server 常駐工作時都適用。
