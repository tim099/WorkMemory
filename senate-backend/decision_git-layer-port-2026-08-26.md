---
id: decision_git-layer-port-2026-08-26
topic: senate-backend
title: git 管理層移植 SCP_Core：四顆拆法、宿主注入、CLI 寫入端不給預設對象
type: decision
status: active
created_at: 2026-08-26
created_by: calli
links: []
related_docs: []
---

UCL_GitSubmoduleSyncPage 的 git 邏輯移植到 SCP_Core，拆成四顆（唯讀/寫入分檔）＋ SCP_ProcessRegistry。

**拍板**
- 不搬檔：那 1225 行裡值錢的是規則，UI 與宿主依賴重寫。UCL_Core 端一行不動（之後改用 Senate 介面）。
- 命名：SCP_Git（唯一 process 出口）/ SCP_GitRepo（唯讀讀數）/ SCP_GitSubmodule（樹＋.gitmodules＋啟發式）/ SCP_GitSync（唯一會改狀態的一層）。讀寫分檔 ⇒ 「這方法會不會動我的工作區」看它在哪個檔就知道。
- registry 由宿主注入（Configure/Log/Warn/CleanupStale）；沒 Configure 就整體停用並喊一聲，**刻意不退到暫存目錄**（退了等於登記在沒人看的地方）。
- Register 的 allowMultiple 預設從 UCL 的 false 翻成 true（singleton 是特例，要顯式要求）。
- git 呼叫掛 registry 走 SCP_Git.Scope 的 [ThreadStatic]：批次在背景執行緒、UI 執行緒可能同時跑自己的 git，共用 static 會讓「收掉上一輪」殺到別人。
- sync CLI **不給預設對象**（必須 --root 或 --project）；--push 另外要 --yes。唯讀的 status 才給預設。
- 寫入端在 CLI 不在頁面：CLI 一次呼叫一顆 process，分鐘級批次塞不進「按鈕那一幀」⇒ 會變成按了沒事的鈕。頁面唯讀但把等價指令印出來。

**踩到的（三顆都是實跑掉出來的）**
1. Branches 把 `origin` 當本地分支：`refs/remotes/origin/HEAD` 的 `%(refname:short)` 是字面 `origin`，不以 `origin/` 開頭 ⇒ 收進本地清單，local 1→2 讓「只有一條分支就用它」的啟發式失效，而失效的樣子是「它挑了另一條看起來合理的分支」。改用 `%(refname)` 全名。（同一顆在 UCL 端由 summit 回寫 f091e611）
2. push 失敗訊息只講「推去哪」：stderr 第一行是 `To <url>`，真原因在後面 ⇒ 加 ReasonLine（成功失敗共用一支）。
3. submodule status 的路徑不能用空白切（資料夾名可含空白，切第二段會拿到半個路徑，然後所有操作對著不存在的目錄跑）。

**驗收方式（可重用）**
本地 bare repo 當 remote 的沙盒 —— 不走網路不需認證，能造出 detached-在-origin-tip、真未合併、dirty、落後、三層巢狀等形狀；三層 push 順序與 gitlink 不變量端到端量到（leaf→mid→root，outer.git 記的 mid SHA 在 mid.git 裡存在）。
