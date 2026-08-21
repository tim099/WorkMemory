---
id: state_2026-08-21-all-shipped
topic: plurk-integration
title: 2026-08-21 收工：七個 op 全通、四則實跑、三格未驗
type: state
status: active
created_at: 2026-08-21
created_by: basecamp
links: []
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/Workflows/Plurk_Maintenance.md]
---

2026-08-21 收工快照。

**已通並實跑（每一則都驗過歸路，不只 200+id）**
- `op=resolve / whoami / lint / preview / upload / post / get`
- 共用帳號 `358451487782338`（limited_to |0|）
- 個人帳號 `358451652874022`（比 owner_id=18166697 ≠ 共用 18174200）
- 含圖 `358451852259674`（兩段式：uploadPicture → URL 併進 content 末行 → 回讀含 `<img>`）
- 公開 `358452026316566`（limited_to 空 ⇒ 公開；「所有人」這條路的首驗）

**仍未驗（事實來源＝Plurk_Maintenance §5，別在別處另記）**
`reply_to` 回應端點（code 有、未實跑）／`公開度=本人` 的 `limited_to=[]`／心情詞完整詞彙表（只對過 12 個）。

**未 commit（在工作區）**：`Cmd_Plurk.cs` 的 `op=get`＋`UnescapeUnicode`、
`UCL_PlurkLint.cs` 的句末誤報修正。

**踩過的坑（都已落成 code 註解或文件）**
Cloudflare 1010 假扮 403（要顯式 UA）／multipart 只簽 oauth_*／ImageReserve 估 30 實測 50／
lint 擋下時回傳檔沒落地（改 try/finally）／lint 誤報 `**` 結尾／`GetString()` 撈不到數字欄位（plurk_id 會變 `?`）。
