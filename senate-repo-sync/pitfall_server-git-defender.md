---
id: pitfall_server-git-defender
topic: senate-repo-sync
title: 經 WMI 起的 Server 跑 git 同步會被 Defender 整顆砍掉
type: pitfall
status: active
created_at: 2026-10-22
created_by: Abigail
links: []
related_docs: [senate:Docs/Workflows/Repo_Sync.md]
---

症狀：開著 sync.tasks 時，`senate server start --detach`（WMI `Win32_Process.Create` 脫樹）起的 main 在 2–70 秒內消失，exit code 0，沒有 ProcessExit、沒有 signal、沒有 console ctrl 事件 —— 從行程裡面量不到任何東西。

原因：Microsoft Defender 行為偵測 `Behavior:Win32/Ransomware!GenRemote.X1`（誤判），action=Remove、由 SYSTEM 執行。證據在事件檢視器 `Microsoft-Windows-Windows Defender/Operational` 的 1116／1117，寫著 senate-server.exe 與那顆的 pid；2026-10-22 一天 20 次逐顆對得上。

對照：同一顆 exe、同一份環境變數改從 shell 起 ⇒ 4 分鐘以上、0 次偵測；只跑 git 的小 exe 兩種起法都不被抓 ⇒ 決定性的是「WMI 起＋Server 跑 git」。

查法的教訓：行程內的診斷全綠時，去看「誰有權限從外面砍它」的那本帳（Defender／EDR 的事件記錄）；一次活下來不算數（第 4 次試跑在觀察窗結束後才被砍）。

⛔ 不做「換一種起法讓它不被偵測」的實驗 —— 那是在探安全偵測的縫（2026-10-22 那個請求被安全機制擋下）。正規的路：向 Microsoft 回報誤判、由人加排除、或改架構（見 decision_git-on-cli-side）。
