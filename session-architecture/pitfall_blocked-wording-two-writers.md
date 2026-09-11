---
id: pitfall_blocked-wording-two-writers
topic: session-architecture
title: 擋下的措辭寫了兩份（同一天、同一個人）—— 而收斂的收據是同源探針，不是比對輸出
type: pitfall
status: active
created_at: 2026-09-11
created_by: basecamp
links: []   # ⚠ 2026-09-11：這裡原本被我填了 senate-backend/none —— 那個 fragment 不存在，而 links 不驗存在。⇒ 寧可空著
related_docs: [task:TASK-0203, commit:52b6b17, commit:0f583ea9]
---

# 同一段措辭寫了兩份 —— 而檔頭早就在警告這個形狀

`UCL_SessionStartGuard` 的檔頭寫著它存在的理由：
> 「**措辭有兩份** —— guard 那份處理了軸1／軸2 的主詞分流，而只要 Coding 直呼 Store，
>  軸2 唯一的消費端就不在 guard 上 ⇒ 她那段是對的但走不到。」

🩸 2026-09-11 做 TASK-0201 時，我在**兩個地方各寫了一次**「範圍擋下的三種理由」：
`SCP_Cmd_Coding.Blocked()`（`SCP_Core 94a1129`）與
`UCL_SessionStartGuard.ReasonOther/ExitOther`（`UCL_Core fb7e86cc`）。
**同一天、相隔幾小時、同一個人。** 兩份都對、都跑得到、而沒有任何一層知道有第二份。

⇒ **被警告的對象在幾小時後照做了**，而抓到它的是 Tim 一句提問，不是我更仔細。

## ⭐ 而收斂的驗收不能靠「兩邊輸出看起來一樣」

⛔ 那證明不了只有一份 —— **兩份各自正確時也會一樣**。

要證它得用**同源探針**：在共用層那句話前面插一個標記（`[0203-PROBE]`），
把**同一個檔**同步到兩個工作副本、兩邊各自重建，然後看誰印出它。
2026-09-11 實測：Unity 入口命中 1、Senate 入口命中 1 ⇒ 改一份兩邊一起變。
（探針跑完 `git checkout` 還原，兩份都回讀確認 0 命中。）

## 可行動守則

> **在共用層加一段「會被兩個宿主印出來」的文字時，收斂的收據是探針，不是比對輸出。**

而**判準是「這一格會不會長出第二個寫者」** —— 不是「它現在有幾份」。
措辭、判定、預設值都算；而宿主**該有的差異**（誰擋你、到幾點、它的收工指令怎麼打）⛔ 不要硬收斂，
硬收斂會做出一個誰都不合身的句子。
