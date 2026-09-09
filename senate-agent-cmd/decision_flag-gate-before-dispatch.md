---
id: decision_flag-gate-before-dispatch
topic: senate-agent-cmd
title: 未宣告旗標 ⇒ exit 2：閘掛 dispatch 之前（TASK-0125）
type: decision
status: active
created_at: 2026-09-10
created_by: basecamp
links: []
related_docs: []
---

## 決策：旗標閘掛在 dispatch **之前**，不是每支子命令各補一次

`senate` 的 `HasFlag` / `ArgValue` 是「找得到就用」的掃描器 ⇒ 打錯的旗標名**不會有人問起它**，
症狀不是報錯而是**安靜地取預設值**（`--wait-reply 300` 打在它上面：畫面正常而它一秒都沒等）。

**修法**（Senate `4863cbf`，publish build id `4863cbf.20260909T144245Z`）：
`Program.cs` 的 `Main` 在 `switch` 之前跑 `RejectUnknownFlags` ⇒ exit 2 ＋ 印出是哪一個 ＋ 那支吃的清單。

⇒ **為什麼不補在各支裡**：補在各支裡的話，下一支新加的子命令天生沒有這道閘，**而那個漏是安靜的**。

### 三個必須一起存在的部件（少一個就會擋掉合法呼叫）
- `FlagsBySubcommand`：每支吃的旗標全集。⚠ **機械抽出來的**（逐函式抽 `HasFlag`／`ArgValue` 的字面），
  ⛔ 不憑印象列 —— 列漏一個＝新擋掉一條本來合法的呼叫。加新旗標時**這張表要跟著加**（漏加是大聲的：自己的新旗標被自己的閘擋下）。
- `ValueFlags`：值型旗標的下一個 token 是**值**，不拿去比對（`--page --window` 那種寫法）。
- `GlobalFlags`：`--no-cleanup` 是**已宣告**而只在 `ui` 底下生效的旗標 ⇒ 照舊「出聲說沒生效然後照跑」（TASK-0123 拍板）。
  ⛔ 不順手改成硬擋 —— 那超出 TASK-0125 的症狀範圍（本單修的是**未宣告**的旗標）。

### 動這種閘之前一定要做的一步
攤既有呼叫端：掃 1779 檔／388 個含 `senate` 呼叫 ⇒ 合法組合 29 種、2153 次全部照舊。
🩸 而名單裡有**一筆真的會壞的**：`ucl-coding` skill 那支「改 C# 進場公告」的**活指令**帶 `--wait-reply 0`
（今天靠靜默忽略才發得成功）。我第一輪把 7 處 `--wait-reply` 全讀成「警告文字」——
**逐筆看才分得出「文件在講這個旗標」與「文件在用這個旗標」。**
