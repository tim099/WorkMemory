---
id: pitfall_seg-ledger-gotchas
topic: streamwatch-segment-ledger
title: 兩格結果未驗＋三個咬人點（段號缺口／章號當合併鍵／.agents 副本）
type: pitfall
status: active
created_at: 2026-08-27
created_by: basecamp
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/StreamWatch_Cmd_Reference.md]
---

接手 0060–0066 這條鏈之前，先讀這幾格 —— 它們是今晚（2026-08-26/27）流血換來的。

## 拍板（只指路，不複製）
- **段序全場共用**、匯出排序讀**後台對照表**不解析訊息本文（Tim 08-26）→ 見 StreamWatch_Cmd_Reference §1.5
- **匯出由「最後收工的人」觸發**（Tim 08-26，取代「只有 primary 觸發」）→ 同上 §1
- **中斷不做接續，直接結算，之後重頭看一輪**（Tim 08-26）→ TASK-0065／0066
- **熱點就是一個時間段，不要 parent/depth**（Tim 08-26）→ TASK-0063、SKILL.md
- **無章名照樣出書（`##None##` 哨兵）**（Tim 08-26）→ TASK-0064

## 🩸 兩格「處置已做、結果未驗」（動這條鏈之前先確認它們還沒被驗）
1. **游標夾 ring buffer 左界**（`StepCycle` 搜 `游標下限夾正`）——
   要「開場時感官水位落後 >40 分鐘」才會走到那條路。**編譯過，沒有實跑讀數。**
   要看的讀數：開場首輪印「游標下限夾正 …… 跳過 N 分鐘」，且**不再**出現連續 20 輪 tiles=0。
2. **最後收工的人觸發匯出**（`SettleAsync` 的 `ActiveGroupPeers`）——
   08-26 那場我是最後一個，路徑走過了；但「**還不是最後一個** ⇒ 印出還在線的是誰」那半**沒被走過**。

## 🩸 咬人點
- **段號在佔段時就發**，而「無新素材」是提早返回 ⇒ 曾經漏寫台帳、留下號碼缺口。
  已修（無素材也寫一行 `tiles=0`）。⚠ 若之後再加任何提早 return 的路徑，**記得同樣補寫一行** ——
  缺號在設計上的意思是「有人佔了段沒交觀察」，跟「這輪沒素材」不可同形。
- **章號當合併鍵會把不同影片併成一章**（TASK-0061 未修）：bilibili 的 work＝UP 主頻道、
  各集獨立，而 `_resolve_from_session` 會把「同章號的舊場次」一起併區間 ⇒
  🩸 實撞：`watch-bilibili-.../002.txt` 把 08-24 那支影片的 9 筆併進今晚這章，
  而讀數全健康（26 筆、633 行）。排序鍵要 **(場次世代, seg_index, tavern_seq)** 三層。
- **`.agents` 的 skill 副本不該與其他三份位元組相同**（安裝器會注入一行 `trigger:`）。
  複製正本過去會把那行吃掉 ⇒ 那支 skill 在 Antigravity 端**不再自動觸發**，而檔案看起來完全正常。
  維護方式：套用同一個編輯，或一次一個 target 跑三次安裝器。
