---
id: pitfall_cross-root-not-found-is-not-absent
topic: streamwatch-segment-ledger
title: 跨 data root：Tasks/Books 共用而 ChatTavern/StreamWatch 不共用 —— 找不到 ≠ 不存在
type: pitfall
status: active
created_at: 2026-08-27
created_by: basecamp
links: []
related_docs: []
---

# 跨 data root：`Tasks`／`Books` 共用，`ChatTavern`／`StreamWatch` **不共用** —— 裂縫上沒有任何一層會報錯

## 讀數（2026-08-27，basecamp wake#76）

`.gitmodules`（AgentCommands submodule 內，共 20 個 submodule）：
- `Tasks` → `github.com/tim099/Tasks.git`、`Books` → `github.com/tim099/Books.git` ⇒ **獨立 repo，跨 data root 共用**
- `ChatTavern`／`StreamWatch`／`_screenstream` 住在 `AgentCommands` 本體 ⇒ **每個 data root 一份**

於是在 `D:/Unity/LY` 這台會看到這個組合：
- 看得到 TASK-0060／0061 的**單子**、看得到 `Books/watch-bilibili-…/002.txt` 的**書**
- 看不到 `StreamWatch/sessions_log.jsonl` 的**台帳**、看不到 `_screenstream/frames` 的**素材**

最短的判別讀數：**tavern seq**。本 root 是 `14548`，而單子裡引用的觀影是 `17040-17087`、
meadow 的投票是 `#17093` ⇒ **兩個不同的酒館**，一眼就分得出來。
第二個獨立證人：`_cmd_results` 全庫 3017 筆，型別清單裡**零筆 `streamwatch.json`**
⇒ `Cmd_StreamWatch` 從未透過這個 Editor 執行過。

## 為什麼它會咬人

單子系統跨 root 共用 ⇒ **一張單會在「沒有它的資料」的機器上被打開**。
而所有的「找不到」在那台都長得跟「資料遺失」一模一樣：
`ls` 沒有、`git log` 零筆、消費端自答 `exists=False` —— **三個來源一致，而三個都只證明了同一件事**
（憲法：兩個來源給出同一個錯不是巧合是共因；這裡是三個來源共用同一個 data root）。

🩸 我今天照這三格寫了一則「產物不在磁碟上」的留言，還列了三格待查的成因假說 ——
**全部歪在同一個方向**，因為我沒問「我現在站在哪個 root」。
Tim 一句「觀影資料應該都在 `_screenstream`」才把我推到那一格。

## 動作型規矩（可以照做的）

1. **凡是「找不到」形狀的結論，句子裡必須留著「在哪裡找的」那個定語。**
   拿掉定語不報錯，它只會讓下一個人以為我搜過全世界。
2. 動 StreamWatch／觀影相關的單之前，先問一句 **「這張單的資料在哪一台」**；
   最快的判別是比 tavern seq，不是找檔案。
3. 只能讀 code、跑不了驗收時，**單子維持原狀、不認領** ——
   「跑不了」與「跑過了」不可以在單上同形。
