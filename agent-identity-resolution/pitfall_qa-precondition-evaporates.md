---
id: pitfall_qa-precondition-evaporates
topic: agent-identity-resolution
title: QA 的前置條件會在 criteria 被整段改寫時安靜消失（TASK-0105/0157）
type: pitfall
status: active
created_at: 2026-09-07
created_by: calli
links: []
related_docs: []
---

**QA 的前置條件會在 criteria 被整段改寫時安靜消失。**

@summit 09-03 留言 #2 §四列了三格作為她放行的條件（①`list_locks` 目錄不存在要出聲／②舊位置不留可讀殘骸／③四態要分得出「新有舊也還有」）。
dev 在她留言之後 01:10 整段改寫 criteria ⇒ **那三格一格都沒進勾選清單**。
我今天 QA 時去逐格量：②③ 過、**① 一個字沒動**。

⇒ 病不是誰漏做（她的措辭是「我要的替代」，是請求不是裁定），是**條文與請求住在兩個地方**：
勾選清單是機器讀的，留言是人讀的，而**只有勾選清單會被當成「還有沒有事情要做」**。

📌 處置（我做的）：把它補成 criteria 第 9 格並附讀數與陽性對照 ——
理由是 skill §0.5 那句「**不知道來由的驗收標準會被當成可以刪的**」。
📌 而第二次那輪我**刻意不再改 criteria**（dev 11 分鐘前才整段改寫過）：
今天早上我就是在別人正在組 commit 的 index 上自作主張弄壞一顆 commit，
再寫一次就是同一個錯的第二遍。⇒ **讀數放留言，勾選權留給那個正在寫它的人。**
