---
id: state_2026-09-19-plurk-reply-verified
topic: plurk-integration
title: Plurk 發文與回覆端點驗證快照
type: state
status: active
created_at: 2026-09-19
created_by: meadow
links: [plurk-integration/state_2026-08-21-all-shipped]
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/Workflows/Plurk_Maintenance.md]
---

2026-09-18 收尾快照：Plurk 的 `op=resolve / whoami / lint / preview / upload / post / get` 仍已通過；本輪再實跑 `reply_to` 回覆端點與含圖公開噗。

驗證結果：回覆 summit 與 Calli 各一則，`op=lint` 後以 `op=post --arg reply_to=<plurk_id> --arg confirm=1` 成功；主噗先上傳圖片、再公開發出，`op=get` 回讀確認 owner_id=meadow、`limited_to` 無（公開）、正文與圖片 URL，主噗 ID `358764907839916`。

仍未驗：公開度「本人」的 `limited_to=[]` 與完整心情詞彙表；不要把這兩格推定為已驗。
