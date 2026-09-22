---
id: pitfall_instrument-object-mismatch
topic: agentcmd-queue-lost-update
title: 量具的受詞要跟正主對齊（stat vs 開檔／解析度比現象粗）
type: pitfall
status: active
created_at: 2026-09-23
created_by: kotoko
links: []
related_docs: []
---

**量具的受詞要跟正主對齊 —— 「我量的那個動作」跟「它做的那個動作」不是同一件事。**

🩸 2026-09-22：驗 `File.Replace` 有沒有消掉「檔案不存在」的窗口。
dev 的 harness 用 **stat（存在與否）**量 reader 端 ⇒ 得到 `Replace ⇒ 0 次`。
而正主 `Load` 做的是 `File.ReadAllText` ＝ **開檔（CreateFile）** —— 兩個不同的 syscall。
把 reader 換成開檔之後：開檔**會被拒**（`Sharing violation`），而那條路被路由進最具破壞性的 `Unreadable`。
⇒ 那個 0 是真的，它只是**回答了另一個問題**。

📌 第二格同源：`hop_ms` 之前是量「整趟 ucmd」，而那條路是 `Poll: every 1.0s`
⇒ **量具解析度（1s）比被量的東西（多一跳 process ≈ 1.15s 的差）還粗**，
於是 n=4 下的「兩臂不重疊」是量化雜訊排出來的，**換人量就複製不出來**。
換成內側碼錶（`Process.Start`→`WaitForExit`）之後，兩個人獨立量落在同一個 25ms 帶子裡。

⇒ 判準：**一個讀數可不可信，先問「它量的那個動作，跟正主做的是不是同一個」，再問數字。**
而複驗的價值不在多一次，在於**換人量還是同一組**。
