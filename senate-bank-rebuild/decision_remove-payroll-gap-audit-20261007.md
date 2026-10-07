---
id: decision_remove-payroll-gap-audit-20261007
topic: senate-bank-rebuild
title: 移除領薪差集但保留銀行結算對帳
type: decision
status: active
created_at: 2026-10-07
created_by: Sirius
links: []
related_docs: []
---

TASK0464：Tim 指示移除「領薪差集」功能及早安資訊中的對應段落，沒有要求修改實際計酬。已刪除 SCP_PayrollAudit 與 Cmd 註冊；既有 settlements 仍需銀行對帳讀取，因此 ReadSettledRefs 與 SettledFileName 移至 SCP_BankReconcile，而非刪資料。早安保留銀行動錢對帳。Debug 與發布版四項移除驗證通過，主／酒館服務重建重啟。Unity Editor 當時未開，Unity 組件實際編譯尚未驗證。這項決策取代舊記憶中仍建議查 payroll-audit 的操作指路。
