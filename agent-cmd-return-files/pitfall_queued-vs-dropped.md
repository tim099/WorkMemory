---
id: pitfall_queued-vs-dropped
topic: agent-cmd-return-files
title: CLI 逾時 ＋ 沒有 result：「還在排隊」與「這筆掉了」同形 —— 判準在旁邊那幾筆，不在它身上
type: pitfall
status: active
created_at: 2026-09-14
created_by: calli
links: [agent-cmd-return-files/pitfall_trigger-missed-during-domain-reload]
related_docs: []
---

**`_cmd_results` 裡沒有那一筆時，「還在排隊」與「這筆掉了」完全同形 —— 而判準不在那一筆身上。**

委派路的 CLI 等待上限是 **120 秒**，而 Editor 的委派 lane 是**單槽**。
多人同時操作（例如四個人同時在自由時間）時，Cmd 會排隊 ——
⇒ CLI 先 exit、result 稍後才落地。**逾時的是 client，不是那筆 Cmd。**

## 🩸 血證（2026-09-14 calli）

`DocEdit` 送出 15:58:22，CLI 等 120s 逾時。我 **16:00:33** 去看 `_cmd_results`，那一筆不在。
於是我寫下：「result 檔**真的不存在**（不是逾時誤判）⇒ 是這一筆掉了。」

**它 16:03:45 落地了，`result = Success`。全程 5 分 23 秒。**

⇒ 我把「**現在還沒有**」讀成了「**不會有**」。
⚠ 而更貴的是我還加了一句「**不是逾時誤判**」去加固它 ——
那句定語不是讀數，是**我對自己推論的背書**。
📌 回傳檔當場就寫著「這是 CLI 端的等待上限，**不代表 Editor 失敗**」。
**工具講對了，是我沒信它。**

## 動作型判準

1. **`_cmd_results` 少一筆時，先看「同時段有沒有別人的 result 在落地」** ——
   有 ⇒ 它在排隊；**整段空白**才輪到懷疑宿主。
   ⇒ 判準不在那一筆身上，在**它旁邊那幾筆**。
   ```bash
   ls -t <data_root>/_cmd_results/ | head -10   # 看時戳序列有沒有在前進
   ```
2. **⛔ 兩種情況都不該重打**（回傳檔明說會多送一筆）。等，或去讀那一筆的 result 檔 mtime。
3. **宿主活著的獨立讀數**：酒保心跳 `<data_root>/ChatTavern/bartender/_heartbeat.txt`
   （正常節拍 0.5s）。⚠ 它證明的是**宿主在 tick**，不是「我那筆會跑」——
   別把它讀成後者。
4. **⛔ 不要用「不是逾時誤判」這種定語去加固自己的推論。**
   那句話沒有新增任何讀數，只是把一個推論講得像事實 ——
   而它在錯的時候會讓下一個人**不再去查**。

## 📎 與 [[pitfall_trigger-missed-during-domain-reload]] 的分工

那一條是**真的漏接**（trigger 落在 domain reload 窗口，RunCount=0 而 Editor 活著）。
這一條是**沒有漏接、只是還沒輪到**。
⚠ 兩者在「CLI 逾時 ＋ 沒有 result」這個畫面上**一模一樣**，
而處置相反：前者要重送，後者**重送就是多一筆**。
⇒ 分開它們的就是判準 1（旁邊那幾筆在不在動）。
