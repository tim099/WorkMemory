---
id: decision_two-layer-gateway
topic: canvas-csharp-port
title: 本體移植、金流委派（Tim 拍板：內部串 ucmd）
type: decision
status: active
created_at: 2026-09-03
created_by: basecamp
links: []
related_docs: []
---

**Tim 2026-09-03 拍板：內部可以串 ucmd，不用全部移植到 Senate。**

⇒ 畫布分兩層，只移植第一層：
- **本體**（events replay／增量快取／RGB332 調色盤／PNG 編碼／note／claim／place 寫入端）→ SCP_Core，三宿主共用
- **付款・自由時間資格・分享** → **不移植**：CLI／Server 走 `AgentCmdClient.Submit+Wait` 派 ucmd 給 Editor；Editor 內直呼既有 ledger

🩸 而我原本的 C 方案是錯的方向：我把「C# 要付款」推成「兩顆 ledger 得移進 SCP_Core」，
於是得出「週級、要動 BankAdminPage 與 Cmd_Sculpture 的共用寫入端、要拍板」。
量了才知道 `AgentCmdClient` 是 **public static 且不綁 `PortStatus`** ⇒ 一支本地 Cmd 可以在中途派一次 ucmd 拿值回來。
⇒ **那一格最貴的成本是我自己推出來的。**

📌 兩個附帶決定：
1. `SCP_CanvasGatewayHost` 是**工廠**（吃資料根當參數），不是現成實例 ——
   閘的根與畫布狀態的根若是兩個來源，不一致時會安靜地把付款派到另一個專案（TASK-0112 那族的預防形式）。
2. Editor 端 gateway 實作**刻意不做**：Unity 那側零個 .cs 引用 `SCP_CmdRegistry` ⇒ 沒有消費端，
   寫了會在沒人驗的狀態下腐爛而單子顯示「做完了」。判準是「它現在有沒有第一個消費端」，不是「遲早用得到」。
