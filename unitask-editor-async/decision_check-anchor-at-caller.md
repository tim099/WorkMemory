---
id: decision_check-anchor-at-caller
topic: unitask-editor-async
title: op=check 的錨要架在呼叫端：expect_text（TASK-0163 reviewer 帶回）
type: decision
status: active
created_at: 2026-09-10
created_by: basecamp
links: []
related_docs: []
---

## 決策：`op=check` 的錨要架在**呼叫端**，不是只架在 handler 內（UCL_Core `20a66b2e`）

`Mutate` 把 RMW 收成單一入口之後，`OpCheck` 鎖內會拿「鎖外那一讀」的原文當錨比對（summit 的設計，`846508ed`）。
**那個設計是對的，但它的跨度只有毫秒級** —— 它保護「本次 cmd 鎖外那一讀 → 鎖內寫」。

🩸 而人的決定來自**更早一次 dry-run**（秒／分鐘級），那份清單**從來不進到這支 cmd 裡**：
> 活體（basecamp 2026-09-09，兩條真 lane：`basecamp`/agent `claude-code` × `Template`/agent `Template`，相距 **31ms**）：
> 意圖是「A 勾甲、B 勾乙」，而 B 的 `criteria_index=2` 落在**丙** —— A 先勾掉甲 ⇒ 未勾清單位移
> ⇒ **一個署名落在呼叫端從來沒選過的那條標準上，兩邊都回 Success。**

⇒ 修法：`op=check` 新增選填 `expect_text=<那一行的前綴>`（多筆用 `|`、筆數要與 `criteria_index` 相同、照使用者順序配對），
對不上 ⇒ 整批不做、零位元組。**形狀照 `senate cmd msg --arg expect_uuid`：序號會位移，文字不會。**

📌 一般化：**「同一個序號在兩次讀取之間指向不同的東西」這一族，守衛必須架在做決定的那一端。**
鎖只能保證「我讀到寫之間沒有人插隊」，它保證不了「我決定的時候看到的是這一行」。

## 踩坑：兩種相反的 0 共用同一句話

驗收標準**沒有 `- [ ]`** 的單，`op=check` 讀數是 `0/0`，而它原本印「（全部都勾了）」
⇒ 看板上長成 `in_review` ＋「全都勾了」＝**看起來已驗完，而一格都沒簽**。
現在分家：「一格勾選格都沒有」會印那一段有幾行文字＋修法。

📌 現況讀數（照 `UCL_TaskIO.ReadSection`／`CriteriaBoxWidth` **同一條規則**數）：**183 張裡 22 張**是這樣。
🩸 而這個數字我數錯兩次：① 我自己的 regex 把區塊切早了（說 30，`0019` 當場否證：工具說 15/1）；
② 我把 `IsSectionHeading` 猜成「`## ` 開頭」，而它是**白名單**（`## 驗收標準/任務描述/結單說明/留言/活動與討論時間線`）。
⇒ 第三版掛三個陽性對照（0019 15/1、0163 3/2、0125 5/2）全部一致才敢寫。
**要複製工具的讀數，就得複製它的邊界規則，而邊界規則不在文件上、在 code 裡。**
