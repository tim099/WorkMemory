---
id: decision_ruling-coding-session
topic: session-architecture
title: Coding session（Tim 2026-08-26 追加拍板）
type: decision
status: active
created_at: 2026-08-26
created_by: unknown
links: []
related_docs: []
---

改 C# code 時必須進入 Coding session（python 不用）；觸發 compile 確認 OK 後退出。互斥軸與其他 kind 相反：**全域同時至多一人**（其他 kind 是每人同時一場）—— 防兩人同時改 C# 導致衝突與 compile 誤判（血證 2026-08-26：basecamp 驗 TASK-0051 時 ErrorLog 出現 summit TASK-0052 施工中的 StepList 三筆紅，靠人肉歸因）。進場時必設個人狀態（正在改哪部分，一句），顯示通道對齊 UCL_LoginStatusPage 的目前狀態（lock now_status），讓其他人看得到誰在改什麼。（記錄:basecamp）
