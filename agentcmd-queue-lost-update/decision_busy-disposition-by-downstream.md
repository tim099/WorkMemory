---
id: decision_busy-disposition-by-downstream
topic: agentcmd-queue-lost-update
title: Busy 的處置照下游選：讀改寫拒寫／錢不猜／健檢 Fail／輪詢下一輪
type: decision
status: active
created_at: 2026-09-27
created_by: kaguya
links: []
related_docs: [Assets/Plugins/SCP_Core/Runtime/Io/SCP_AtomicFileRead.cs, task:TASK-0265, commit:ae9cb58, commit:0bfad45b, commit:a7b867a]
---

**讀取端護欄的處置怎麼選（TASK-0265，2026-09-27）** —— 唯一實作是 SCP_Core `SCP_AtomicFileRead`（UCL_AtomicFileRead 轉呼叫它，⛔ 別再抄一份）。

重試（2+4+6+8 ms）關掉的是**換檔那一瞬間**這個主成因；**重試用完仍 Busy** 時，各處的處置是逐類選的，而選法有一條規則：

| 讀取端的下游 | Busy 的處置 | 例 |
|---|---|---|
| 讀改寫（讀完要寫回） | **拒寫**、丟例外或回 false | queue 寫入路徑、SCP_Prefs.Mutate、Demurrage AppendIssued、RateCache → 呼叫端 oError |
| 決定錢去哪 | **Unresolved／不做**，⛔ 不往下猜 | Resolver `s_UnreadablePersonas`、GetBankAccount 不借別區、Demurrage 名單殘缺整批不發 |
| 健檢／稽核 | **直接 Fail「重跑」** | bank-audit（殘缺輸入算出的是形狀正確的錯答案） |
| 型別只有一個值、呼叫端多 | 回預設但 **oWhy 明說「這不是本區的值」** | SCP_BankRegion.Read／BankPolicy.Read |
| 唯讀輪詢 | **下一輪再看**（回 null），只警告一次 | Senate AgentCmdClient 輪詢 |
| 只影響顯示 | 重試後照舊 | render_settings、Morning.LoadRegistryMeta |

⚠ 快取：本輪有人讀不了 ⇒ **不落快取**（`s_Loaded = !aIncomplete`），否則那一位整個進程壽命都錯。
⚠ 訊息：Busy 叫人**重跑、別動檔**；壞檔叫人**先備份再修、別直接刪**。兩者一樣會讓人刪掉健康的檔。
