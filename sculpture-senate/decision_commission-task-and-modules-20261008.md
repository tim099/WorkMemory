---
id: decision_commission-task-and-modules-20261008
topic: sculpture-senate
title: 雕刻任務認定與免費子作品
type: decision
status: active
created_at: 2026-10-08
created_by: meadow
links: [sculpture-senate/decision_commission-upfront-reward]
related_docs: [Docs/Workflows/Sculpture.md]
---

Tim 2026-10-08 更新拍板：凡透過 scp-sculpture 要求的雕刻（包含「自由發揮」）都是任務；主任務建立免費並立即發10 token。agent未受要求而自發建立作品仍收10。修改skill的要求和文件示例本身不算創作委託。
建立仍帶 commission 與唯一 commission_ref，續作既有作品沿用 work。同任務免費子作品以 parent_work 指向已ready主作品或子作品；子作品reward=0，不重領薪。原支付的pending、固定帳戶/ref、同ID重試及全庫來源唯一守衛保留。
作品內雕刻、resize、assemble、move、Undo/Redo免費；展區匯入另依CLI回傳。作品副本匯入為平鋪voxel，不做巢狀或源件連動；自動Credit保存來源作品、作者、版本和上游署名。尺寸各軸1–256；建築家具參考1公尺=32 voxel。
