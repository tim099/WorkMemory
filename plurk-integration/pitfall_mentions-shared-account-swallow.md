---
id: pitfall_mentions-shared-account-swallow
topic: plurk-integration
title: op=mentions 吞掉同帳號室友指名我的回應（TASK-0153）
type: pitfall
status: active
created_at: 2026-09-15
created_by: kiara
links: []
related_docs: []
---

**根因是一個 `continue`。**（UCL_Core `ecb7db19`）

```csharp
if (aUid == aMeId) { ...; continue; }   // 它從來沒問過「這一則有沒有指名誰」
```

⇒ **「這則是本帳號發的」與「這則指名我」被寫成互斥**，而第一個先判、直接 continue。
共用帳號底下室友跟我**同一個 user_id** ⇒ 她指名我的那一則走進這分支就再也出不來：
`SignedBy(我)` false（她署名自己）、`aUnsignedMine` 那格要求 `!Found` 而它 Found=true
⇒ 三個出口一個都不符，**兩個桶都沒進，而 💬N 知道它在**。

## 射程：不是一筆，是 13 列（limit=30 候選窗）

| 層 | 筆數 | 舊版 |
|---|---:|---|
| 本帳號發的、指名我的**回應** | 7 | 全部被吞 |
| 本帳號發的、指名別人的回應 | 6 | 全部被吞 |
| 本帳號發的、指名我的**噗本體** | 4 | 一直看得見（噗本體走另一條路，沒這個洞） |

其中 2 筆當時仍未回 —— **室友指名我說的話，而我不可能知道它們存在。**

## 守衛：歸桶對帳**每次都印**（含全中那次）

數「含 @ 本帳號的回應」與「真的被歸桶的」，不平時印 🔴 並給下一步。
⛔ 只在出事時印的話，「這次沒漏」與「守衛根本沒跑」在回傳檔上同形 —— 那是本單這隻病換一張臉。

## ⛔ 沒做（不同修法 ⇒ 不塞進同一張單）

留言 #1 那格：**alerts 沒有噗 id**（Tim 09-07 拍板過該給）。要改 `op=alerts` 印法，
且得先驗 API 到底給不給（而預設那條路 `getActive` **讀了會清通知**）。
