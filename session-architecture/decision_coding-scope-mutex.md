---
id: decision_coding-scope-mutex
topic: session-architecture
title: Coding 場從「同時至多一人」改成「範圍撞到才擋」（純路徑判準 / 登記表不搬 / claim 開場）
type: decision
status: active
created_at: 2026-09-11
created_by: basecamp
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Session_Kinds.md, task:TASK-0201, task:TASK-0202, task:TASK-0203, commit:94a1129, commit:fb7e86cc, commit:d1aad3c4]
---

# 全域互斥不再是「同時至多一人」，而是「範圍撞到才擋」

**拍板（Tim 2026-09-11）：純路徑判準 —— 同一個 repo 的兩份工作副本視為不衝突。**

`D:\Unity\Senate\SCP_Core` 與 `D:\Unity\LY\Assets\Plugins\SCP_Core` 是同一個 repo 的兩棵樹，
本判準**不算衝突**。⛔ 代價已知且被選擇：兩人各改一份副本的同一支檔時**這道閘不會叫**，
失效樣子是各自 commit 後 push 分叉 —— 要到 push 才現形。

📌 反過來說，這正是**「SCP_Core 一律改 Senate 那份」**的理由（同一天拍的另一個板）：
Senate 是獨立 repo、獨立編譯，**不被 Unity 側的施工場排隊**。
⛔ 要在 `SCP_SessionScope` 補 repo 身分解析，得先翻這兩個板其中一個。

## 三態不是「衝突／不衝突」

`SCP_SessionScope.BlockKind` = `MineUnset` / `TheirsUnset` / `Overlap`，
因為**三者的處置相反**：我自己補得了的／只有對方補得了的／要等或協調。
而 `ExitOf` **只有 `MineUnset` 回非空** —— 那是唯一你自己補得了的一種，
它要排在「等他」**前面**，不然讀的人會照著「等」去等一件他本來不必等的事。

## 缺欄位不可以讓既有的閘失效

兩側任一沒宣告範圍 ⇒ **退化成舊行為（全擋）**。
⛔ 反過來（缺欄位就放行）會讓舊 session 檔在升級那一刻**靜默失去保護**，
而症狀是兩個人同時進場、各自以為自己是唯一。
⚠ 而「範圍解不開」≠「沒宣告」：打錯路徑當場 exit 2，⛔ 不靜默退化
（靜默退化會讓打錯的人拿到一個他沒要的全域鎖）。

## ⛔ 為什麼 kind 登記表**沒有**一起搬進 SCP_Core（TASK-0203 ① 拍板）

`UCL_SessionKindEntry.SettleResidueAsync` 的型別是
`Func<…, UniTask<bool>>` —— **UniTask 是 Cysharp，Unity 專屬**。

⇒ 登記表要過去，得先把「關場結算」的契約重寫成共用層表達得了的形狀，
**而結算就是金流，而金流不搬（TASK-0106 Tim 拍 B）**。
⇒ 為了收斂一段措辭去動一個已拍板說不動的東西 —— 不划算，而症狀根本不需要它。

📌 **「全面遷移」那個更大的題目沒有被吃掉**，它現在有明確前提：
**先讓結算契約不依賴 `UniTask`**。要做就另開一張單。

## 開場的時機掛在「動工一張單」上（TASK-0202）

`op=claim --arg scope=<絕對路徑>` ＝「我現在要動工」⇒ 認領＋開場＋綁單，**而且原子**：
開場被擋時**認領一個位元組都不寫**；`Mutate` 失敗時**把剛開的場收掉**（回捲失敗要出聲）。
不帶 `scope` ＝「我先記錄我在做這件事」⇒ 行為一格不變。

⛔ 不做成必填：`claim` 同時回答「誰在做」與「我開始動手」，而**這兩件事常常差好幾天**，
很多單根本不改 C#。把便宜的動作變貴，貴的動作就會被繞過。

## ⚠ 接手前要知道的三格

1. **編譯閘量的是整棵樹** ⇒ 多場並行時它**分不出紅字是誰造的**（退場輸出會印同時在場的人）。
   ⛔ 別拿它當 `force=1` 的理由，也別替別人改。
2. **`op=resolve` 不會觸發自動收場** —— 自動收場掛在 `senate cmd commit` 之後。
   走 resolve 結單的話，場要手動 `step=end`。
3. **這條線的單（0201/0202/0203）全部 done，而主題留在 `active`** ——
   因為 `archive` 的 git 前置守衛要求內容真的在版控裡，而本 fragment 還沒 commit。
   ⇒ commit 落地後可再 `archive`。⛔ 那不是我的待辦（提交由 Tim 收尾）。
