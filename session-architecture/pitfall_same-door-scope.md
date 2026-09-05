---
id: pitfall_same-door-scope
topic: session-architecture
title: 「走同一個門」的兩次射程誤用 —— 不是所有路徑；而「同一個函式」≠「同一條路」
type: pitfall
status: active
created_at: 2026-09-05
created_by: basecamp
links: [session-architecture/pitfall_mechanism-exists-nobody-walks-it]
related_docs: [task:TASK-0055, task:TASK-0057, commit:092dd940, commit:7e5d8a1]
---

「所有關場路徑走同一個門」這句話今天咬了兩次，**兩次都是射程問題**。

## ① 射程不是「全部路徑」，是「原本沒有結算的那些」

2026-09-05 早上我在 TASK-0055 加了一格驗收：「兩個 kind 的收工路徑改呼叫 `CloseWithSettlement`」。
**下午量出來那格是錯的、撤回了**：那兩條路**已經先結算再 `Close`**
⇒ 再走一次統一入口就是**第二次結算**（觀影場會**重複發薪**）。

⇒ 判準：**通則要問適用範圍**。這條的適用範圍是「補收工／管理頁那些原本跳過結算的路」，
不是「每一條會關場的程式碼」。

## ② 「同一個函式」跟「同一條路」不是同一句話

我在 `Session_Kinds.md` §5 寫「晚安自動關**走的是同一條補收工路**」。
@summit 照它推出「E 對 Coding 走不到」（因為補收工的射程是**殘留**，而 Coding 沒有 `end_ts` ⇒ 永遠不是殘留）。
**她推得沒錯，是我的句子錯了**：晚安走的是同一個**函式**（`UCL_SessionCloseFlow`），
而**判準不同** —— 補收工要「殘留」，**晚安只看 `active`**。
⇒ 對 Coding：**補收工到不了，晚安到得了。**

## 動工時照做

寫「A 和 B 走同一條路」之前，先分開回答兩句：
1. **同一段程式碼嗎？**（函式層）
2. **同一個觸發判準嗎？**（射程層）
兩句都成立才叫「同一條路」；只有第 1 句成立時要寫成「**同一個函式，不同判準**」。
🩸 混寫的代價不是抽象的：它讓一個讀文件的人推出一個**正確推理、錯誤前提**的結論，
而那種結論看起來完全站得住。
