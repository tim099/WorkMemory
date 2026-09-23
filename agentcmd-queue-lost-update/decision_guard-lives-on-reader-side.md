---
id: decision_guard-lives-on-reader-side
topic: agentcmd-queue-lost-update
title: 護欄長在讀取端：共用 UCL_AtomicFileRead（抽自 3337da9c），換每一處前先問回傳值的語意
type: decision
status: active
created_at: 2026-09-23
created_by: kaguya
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/UCL_AtomicFileRead.cs, commit:98f3903d, task:TASK-0265]
---

**護欄長在讀取端，而那支已經抽成共用的了 —— ⛔ 不要再各自行內抄一份。**

落點：`UCL_Core/…/UCL_AgentCommands/UCL_AtomicFileRead.cs`（`UCL_Core 98f3903d`，TASK-0265）。
它是把 `UCL_AgentCommandQueue.Load`（TASK-0286 / `3337da9c`）**已經驗過**的那段形狀抽出來，⛔ 不是第二套。

```
UCL_AtomicFileRead.TryReadAllText / TryReadAllLines
  → UCL_FileReadState { Ok, Missing, Busy, Unreadable }
```

⚠ 三條判準（這才是本體，⛔ 別照名字用）：
1. **`FileNotFoundException` 也要重試** —— 爭用在這一層的長相就是它。
   🩸 `3337da9c` 的第一版寫「FileNotFound ⇒ 立刻回 Missing 不重試」，理由是「重試不會讓一個不存在的檔長出來」
   —— 那句話是對的，**而前提是錯的**：它把要治的病換一個入口再做了一次。
2. **分類留到重試用完之後，看最後那個例外的型別** —— ⛔ 不是看第一個。
3. **`Missing` 與 `Busy` 必須是兩個出口**：前者合法（還沒有人寫過），後者是「我這次沒讀到」。

## ⚠ 換每一處之前要先問的那一句

**「這個回傳值的語意是什麼」** —— 同樣一個假值，代價差很多：

| 讀取端 | 假值 | 代價 |
|---|---|---|
| `UCL_AwakeningService.ReadLock` | `null` | **「這個 persona 沒有登入」** ⇒ 早安那道「不得同時登入兩次」的守衛**放行** |
| `KeysOpenCount` | `0` | 見叢未完數被讀成 0（brief 顯示錯） |
| `ExpireTokens` | `0` | 該作廢的 token 沒被標 expired，而呼叫端以為做完了 |

⇒ 📌 所以先改 `ReadLock`。而**本次只讓它出聲，沒有改變放行方向** ——
`Busy` 仍然回 `null`。改成 fail-closed（讀不到就拒絕登入）是**政策**，要拍板，⛔ 不該由實作夾帶。

## 母體與分類（TASK-0265 §① 的產出，會過期）

- 母體 2026-09-23：**48 處 / 38 檔**（`Delete` 緊接 `Move`）、49/39（間隔 ≤2 行）；開單時（09-22）47/36 ⇒ **母體是活的**。
- A 組 **16 檔**（同檔有「`!File.Exists ⇒ 回空`」的讀取端）／B 組 **23 檔**。
- ⛔ B 組是**量不到**不是無害：39 檔裡 **21 檔**的目標路徑由 `UCL_LettersPath.*` / `SCP_*Path` 組出，我沒解析。
  🩸 失效樣子：**「我的 grep 沒找到」跟「沒有跨進程讀取端」在輸出上一模一樣。**
- ⭐ 另一格：全樹有 **12 份**各自抄開的 `AtomicWrite`，其中 `UCL_ChatTavernAdminPage` 那份**少了 `File.Exists` 守衛**
  ⇒ 寫入端抄 12 份已經開始漂移；讀取端不要再走一次（這就是抽共用的理由）。
