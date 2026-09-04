---
id: pitfall_cross-process-delegation
topic: session-architecture
title: 跨 process 委派四格：Editor 開著變條件／回讀才是判定／UI 不能同步等／lane 與 target_persona
type: pitfall
status: active
created_at: 2026-09-04
created_by: basecamp
links: []
related_docs: []
---

2026-09-04 做 TASK-0127 ④⑤⑥ 時，關於「跨 process 委派」的四格。

## ① 「Editor 開著」從前提變成條件

補收工原本住在 Editor 裡 ⇒ Editor 一定開著。搬到 Senate 之後，Editor 沒開就 `exit 3`（`not_running`，**刻意不降級**）。
⇒ 頁面／CLI 要顯示得出這個狀態，不能讓人按下去拿到一個**看起來像壞掉**的錯誤。
⚠ 這格目前**沒有活體讀數**（要關 Editor 才測得到，而 Editor 有人在用）—— 留給 QA。

## ② 委派 ≠ 判定：判準永遠是回讀磁碟

gateway 回 true 只代表「**那一端說它做了**」。它在另一個 process 上，而回報字串會替自己說謊。
⇒ `CloseWithSettlement` 在 gateway 回來之後**一定回讀** `Load()`，用磁碟的 `active` 當 `Closed`。
📌 同族：09-03 的「回讀跟寫入共用同一個錯的根 ⇒ 回讀綠不是證據」——
這裡剛好相反：**兩端不同源，所以回讀才是證據**。

## ③ UI 迴圈不能同步等 1〜3 秒（而現在沒有機器會抓）

關場是檔案協議 ＋ Watcher 輪詢。會重畫的宿主同步等 ＝ **視窗凍住**，
而 `ui --soak` 的凍窗閘 2026-09-04 已退場（Tim 拍板，改成收尾開常駐窗）⇒ **凍住沒有機器抓得到**。
⇒ `SCP_GuiSessionAdminPage` 用 `SCP_GuiHost.RedrawsContinuously` 分兩條路：
會重畫 ⇒ 背景 `Task` ＋「⏳ 委派中」態；不會重畫（CLI 單次 render）⇒ **同步跑**
（那裡沒有第二幀可以觀察結果，背景 task 等於把答案丟掉）。

## ④ lane 選誰：刻意用**目標**的 persona

`SenateSessionCloseGateway` 送 Cmd 時 lane ＝ 被關的那個人。
好處：同一場的兩次關場請求會在同一條 lane 串行（併發關場自然不可能）。
代價：Editor 的回傳檔落在**目標的** letters 夾（那份報告講的正是他的場，可接受）。

⚠ 而同一族的另一格是**參數命名**：`--persona` 與 `--arg persona=` 是**同一個 key**
（`AgentCmdClient`：兩者不同時依 `--arg` 值送出）⇒ 目標若也叫 `persona`，呼叫者身分會被蓋掉。
🩸 實撞：回傳檔落進目標的夾、還替一個不存在的 persona 長出 `letters/` 目錄。
⇒ **目標一律叫 `target_persona`**（`Cmd_Task` 既有慣例）。
