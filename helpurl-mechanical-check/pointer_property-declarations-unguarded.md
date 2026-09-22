---
id: pointer_property-declarations-unguarded
topic: helpurl-mechanical-check
title: 未完線：47 處 property 宣告沒有機械檢查在守
type: pointer
status: active
created_at: 2026-09-22
created_by: kiara
links: []
related_docs: [commit:f2d6cafb, task:TASK-0257]
---

**未完線：`override string HelpURL` 那一族（47 處）目前沒有任何機械檢查在守。**

全專案只有一個基底宣告它：`UCL_AgentCommandHandlerBase.cs:57 public virtual string HelpURL => ""`
⇒ 那 47 處**全部是 AgentCommand handler**，權威清單走 Registry 自己的 `ExportCommandCatalog`（反射），⛔ 不 grep。

2026-09-22 追了一趟：47 條／相異 45 條 ⇒ **缺 4**（5 支 Cmd 的「查看說明」是死的，含每天在跑的 `GoodMorning`）。
兩族成因：文件搬進 `Plan/completed/` 沒同步（3 支）／文件從來沒寫過（2 支）。已修好 4 條（`commit:f2d6cafb`）。

⛔ **而修好不等於被守住**：`Cmd_HelpUrlCheck` 只掃 attribute ⇒ 任何人再把一份文件搬一次，這四條就會再壞一次，
而它一樣不會叫。⇒ 要擴口徑的人：桶子加在 `Scan()` 旁邊，清單問 Registry 要，
**解析仍然走 ResolveURL 本人**（見本主題的 decision 那格）。

⚠ 存在性我當天是用**自己搭的尺**量的（ucl_core: → core 根、{lang}→zh-Hant、缺檔退 en），
並拿 ResolveURL 本人抽樣對拍 5/5 逐字相同（4 陰 1 陽）—— 下一個人要嘛照做，要嘛直接把它做進 Cmd 裡。
