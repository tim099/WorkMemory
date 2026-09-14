---
id: decision_why-rebuild-not-port
topic: senate-bank-rebuild
title: 重做不搬：單子上的前置已過期、臨界區比寫的小、而唯一寫入端本來就存在
type: decision
status: active
created_at: 2026-09-14
created_by: basecamp
links: [treasury-bank-hardening]
related_docs: [task:TASK-0209, task:TASK-0106, Assets/Plugins/SCP_Core/Runtime/Canvas/SCP_ICanvasGateway.cs]
---

**為什麼是「重做」不是「搬」**（Tim 2026-09-14 拍板，而底下的讀數是拍板前量的）

舊路線是把 `UCL_TreasuryLedger` 那 10 檔 3,775 行從 Editor 搬進 SCP_Core。三格讀數推翻了它的前提：

1. **單子上寫的前置已經不存在**：0106 說「綁 `UCL_Asset` ⇒ 搬入前設定層要先換 `SCP_Prefs`」——
   而 Treasury 那 10 檔**零個 ScriptableObject／UCL_Asset**（全是 static class ＋ JSON），
   且 `SCP_Prefs` 早就在 `SCP_Core/Runtime/Prefs/`。那段是 09-02 的讀數，12 天就過期了。
2. **真正的臨界區比單子寫的小**：ledger 是 per-entry 檔（檔名帶 uuid）⇒ append 天生無碰撞，
   `Credit` 不需要互斥。只有 `Debit` 的 `GetBalance → 比大小 → WriteEntry` 是 TOCTOU，
   而守它的 `s_BalanceCacheLock` 是 **in-process lock，保護的是記憶體 dict 不是那個序列**。
3. **今天其實已經只有一個寫入端**：`SCP_ICanvasGateway` 檔頭白紙黑字 ——
   券／token 的 canonical ledger 權威實作只有 Editor 那側，CLI／Server 走檔案協議派給它。
   ⇒ 這件事不是「建立一個不存在的序列化」，是**把那個唯一寫入端從 Editor 換成 Server**。

⇒ 重做之後，要遷的只有**餘額**（一筆 `opening_balance` 分錄），不是三千多行邏輯。

**射程**：新舊完全分開 —— 新的住 `<bankRoot>/`，舊的 `Treasury/` 凍結為唯讀歷史，⛔ 不刪不搬。
