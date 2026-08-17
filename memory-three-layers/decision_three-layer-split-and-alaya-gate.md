---
id: decision_three-layer-split-and-alaya-gate
topic: memory-three-layers
title: 三層分工判準 ＋ Alaya 門檻改為「一個人認為就整理」（人數＝權重）
type: decision
status: active
created_at: 2026-08-17
created_by: calli
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Memory_Common_Principles.md, ucl_core:Docs~/zh-Hant/Workflows/Alaya_Collective_Memory_Workflow.md, ucl_core:Skills~/ucl-memory/SKILL.md, repo:Docs/Plan/Plan_Collective_Subconscious.md]
---

Tim 2026-08-17 兩次拍板，第二次推翻第一次落地後的設計。

## 三層分工（判準寫成可執行的問句）

```
① 這條沒有「我」也成立嗎？   不成立 → 個人記憶  letters/<persona>/fragments/
② 它綁在某一項具體工作上嗎？  是 → 工作記憶      AgentCommands/WorkMemory/<topic>/
                              否 → 集體潛意識    AgentCommands/Alaya/fragments/
```

不確定 → 預設落點是個人層（升級好搬，反向降級會讓外部 links 斷）。

## Alaya 入庫門檻：**一個人認為就整理**（人數是權重不是入場券）

初版（`63c4874`）設「兩位以上 persona 各自栽過才准進」，當天被 Tim 推翻（`d9dd3a7`）。

**錯在哪**（三條，寫進 workflow §3 而不只在 commit）：
1. `lessons.jsonl` 的病**不是入庫太寬，是沒有維護** —— 200+ 筆就算每筆兩人認證一樣沒人讀得完。
   **一次性的閘擋不住持續的增長。**
2. 「等第二個人栽」＝**把門檻成本轉嫁給同事**（要真的讓下一個人先撞一次）。
3. 通用性判斷**不需要樣本數**。

⇒ 人數改記為 `recurrence`＝**被回憶到的權重**。多人踩到就 +1、`links` 加上對方。

## Alaya 刻意不做 daemon / 不做自動偵測 / 不需背景排程

理由是前代 Collective_Subconscious 的死法（見 `repo:Docs/Plan/Plan_Collective_Subconscious.md` §4）：
**它是一個「只在被呼叫時才作用」的機制，排程它的工具退場之後，它的生死就跟自己的品質無關了。**
⇒ Alaya 的心跳掛在「每個人寫記憶前都會先搜」這個**已經是紀律的動作**上。

## 與既有機制的分工（不要搞混）

| | `lessons.jsonl`（agent-lessons-log） | Alaya |
|---|---|---|
| 形狀 | append-only jsonl | 一檔一主題 markdown |
| 寫入 | 撞到當下立刻 append | **整理過**才進 |
| 維護 | 無（只增不減） | 定期整合，刻意不讓線性成長 |

**上下游不是競品**：jsonl 是進料，Alaya 是成品。
