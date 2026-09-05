---
id: decision_ruling-coding-session-scope-a
topic: session-architecture
title: 拍板：Coding session 射程走 (A) 兩宿主都納入（Tim 2026-09-05）—— 09-04 的成本估計被 0127 的地形推翻
type: decision
status: active
created_at: 2026-09-05
created_by: basecamp
links: [session-architecture/pointer_port-0127-after-onecut]
related_docs: [task:TASK-0058, commit:97f1e3a, commit:a818ddb]
---

Tim 2026-09-05 拍板：TASK-0058 的 Coding session **走 (A)：兩個宿主都納入**
（Unity 那側 ＋ Senate／SCP_Core 那側，改 `.cs` 都要進場）。

## 為什麼這個拍板值得記（它推翻的是一個「算過成本」的舊結論）

2026-09-04 我列三條路時把 (A) 標成「最貴 —— 要跨 repo 的鎖」，並傾向 (B) 或 (C)。
**那個成本估計今天被讀數推翻了**，而推翻它的不是新資訊，是 0127 改變了地形：

- session 層在 0127 ⑦ 之後住 `SCP_Core/Runtime/Session/`，而 SCP_Core **同時掛在兩個宿主底下**
  （`Senate/SCP_Core` 與 `Bar/Assets/Plugins/SCP_Core` 是同一個 remote 的兩份工作副本）。
- 兩個宿主的 `DataRoot` 是**同一個**：`senate cmd sessions` 從 Senate 這側印的就是
  `D:/Unity/Bar/AgentCommands`，而且讀得到 Unity 寫的那 8 份場。
- ⇒ 「跨 repo 的鎖」這筆成本**不存在**：場是同一棵樹上的同一個檔位，
  互斥機制 `SCP_ActivitySessionStore.TryStart` 也已經在共用層（selftest 每次 build 跑三格）。

📌 一般形：**成本估計會因為別張單的交付而過期，而它過期時不會有人來通知。**
⇒ 隔了一段時間才要拍的單，先問「這個估計是在哪一版地形上算的」。

## (B) 為什麼被否 —— 它的失效樣子不報錯

🩸 活體（2026-09-05，basecamp）：我改 `SCP_Core/Runtime/Gui/*.cs`（`97f1e3a`）與
`Senate/src/*.cs`（`a818ddb`）、build 兩次、驗完，**全程 Unity 的 ErrorLog 一個字都沒動**，
直到 `git pull` 進 Bar 那份跑 recompile 才進編譯。
⇒ (B)（只擋 Unity 側）底下，那段時間「Unity 側沒人持場」是真的、
「同一份 code 正在被改」也是真的，**而且兩者都不報錯** —— 跟沒有這張單的世界同形。

## (C) 為什麼被否 —— 便宜路有已知反證

(C) 的理由是「從檔案路徑就能歸因誰的紅」。反證是 2026-09-04 的 `CS1704`：
**報在 UCL_Core，成因在 SCP_Core** ⇒ 症狀出現的位置不等於成因的位置。

## 動工時會咬人的兩格

1. **退出閘的尺兩邊不同形**：Unity＝`check_compile`（tracker ＋ ErrorLog 對帳）／
   Senate＝`build.sh` 出廠驗收。⛔ 不要找一把共用的尺 —— 硬湊會讓其中一邊量的不是它自己的編譯。
2. **Senate 側進場不能依賴 Editor**（那側常常沒開 Editor）。
   做得到：`sessions` 這支是**本地執行**不是 ⤷Unity。
