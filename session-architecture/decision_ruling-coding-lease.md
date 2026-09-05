---
id: decision_ruling-coding-lease
topic: session-architecture
title: 拍板：Coding 場一律帶租期（(A)）＋三個細節；⚠ 而重複進場會把它抹掉
type: decision
status: active
created_at: 2026-09-05
created_by: basecamp
links: [session-architecture/decision_ruling-coding-session-scope-a]
related_docs: [task:TASK-0058, commit:0ea01ef]
---

PM basecamp 2026-09-05 拍板（Tim 當日授權）：**Coding 場一律帶租期 `end_ts`**。

## 為什麼（@summit 發現、我複驗）

`IsRunningAt` 在 `end_ts` 解析不出來時**回 true**（刻意：寧可誤判「還在」）。
⇒ 沒有 `end_ts` ＋ 補收工射程是「殘留」 ⇒ **這種場永遠是「進行中」，永遠不會落進補收工那條路。**
🩸 而 `Coding` 是**全域獨佔**的 ⇒ **持有者掉線就永遠擋住所有人**。

⭐ 我複驗時撿到更難看的一格：被擋時印的出口寫著「**或等它到期之後再跑本 Cmd**」——
而它**永遠不會到期**。**一條讀起來合理、而且永遠不成立的指路。**

## 拍板的三個細節（缺一個就會被繞過）

1. **`until` 有預設不必填** —— 施工不該先要求人回答「我要改多久」。
2. **續期掛在 `op=status`** —— 那一步本來就要跑（場中更新狀態），
   ⇒ 續期長在**必經路上**，不是長在「記得去續」。
3. **到期不等於自動釋放** —— 到期只是落回「殘留」；別人要搶場仍得顯式
   `sessions --arg op=close --arg confirm=1`（寫別人的檔、留痕跡）。
   ⇒ 原持有者還在改時**不會被靜默奪場**，奪場是一個有名有姓的動作。

## ⚠ 而這個保護目前**可以被一個看起來無害的動作拆掉**

2026-09-05 活體：Senate 側開場（帶 `end_ts`）⇒ 接著跑 Unity 側 `step=start`
⇒ **回 ✅ 進場**，回讀檔 `end_ts` 與 `until_local` **變成空字串**。
成因：軸1（每人一場）設計上**不管同 kind**，而 `Coding` 自己的同 kind 守衛**不存在**。
⇒ **租期被抹掉，(A) 的保護就沒了。** 修法在 TASK-0058（`step=start` 加同 kind 守衛）。

📌 一般形：**一個靠「開場時寫對值」建立的保護，會被「再開一次場」拆掉** ——
除非重複開場本身被擋。
