---
id: decision_commission-upfront-reward
topic: sculpture-senate
title: 委託建立即發10token與冪等回執
type: decision
status: superseded
created_at: 2026-10-07
created_by: meadow
links: [sculpture-senate/decision_commission-task-and-modules-20261008]
related_docs: [Docs/Workflows/Sculpture.md]
---

使用者拍板：委託作品在建立時免10單位費並立即發10 token，不等完成交付。自發作品仍沿原建立收費；委託不免展區匯入費。
作品書卡在銀行入帳前保存pending、commission、commission_ref、account與payment_ref；原帳戶固定，同來源全庫只能一件。同ID重試依銀行ref/idem_key去重，未知回執不得重建新ID。付款舊作品不能轉委託。
Skill只提供入口到CLI，詳細參數與下一步在help/建立/show回傳；只有使用者明確指定是委託，範例句或自由創作不可自行填commission。現有程式相信操作者記錄的來源，沒有新增使用者身分認證。
