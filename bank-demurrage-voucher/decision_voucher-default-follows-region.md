---
id: decision_voucher-default-follows-region
topic: bank-demurrage-voucher
title: 券種預設＝區域 id、比例預設 1（Tim 改判），對價是 VoucherTrace 出處欄
type: decision
status: active
created_at: 2026-09-23
created_by: kaguya
links: []
related_docs: [Assets/Plugins/SCP_Core/Runtime/Bank/SCP_BankPolicy.cs, docs/Glossary/default-is-naming-right.md]
---

**Tim 2026-09-22 拍板**：`voucher_type` 缺格 ⇒ **區域 id**（BTC 區發 BTC 券）；`ratio_per_token` 缺格 ⇒ **1**。
⇒ 關掉的手段變成**顯式**寫一格（`voucher_type: ""` 或 `ratio_per_token: 0`），⛔ 不再是「什麼都不寫」。

⚠ 實作要點：判「有沒有這一格」用 `aJd.Contains(key)`，⛔ 不是拿 `GetString` 的回傳值比空字串 ——
顯式清空（我要關掉）與整格不存在（沒人設定過）在回傳值上同形。
⚠ 三條「讀不到設定檔」的早退也要帶同一組預設（共用 `ApplyVoucherDefaults`）——
只在成功那條補的話，「設定檔壞掉」會安靜地變成「不發券」。

🩸 **這是推翻前一版判準**：舊註解寫「預設必須是那個不會造成任何後果的值」，理由是
『沒有人設定過』與『有人決定要發』會在 `VoucherEnabled` 上同形，而後果是憑空鑄券。
**那個顧慮沒有消失** —— 對價是新增的 `Reading.VoucherTrace`：逐格說明券種／比例是
`區域預設`／`設定檔`／`設定檔的值不合法⇒關`，報告與廣播都印它。⛔ 別把它當裝飾拿掉。

落地：`SCP_Core a295367`。詞條：`docs/Glossary/default-is-naming-right.md`（預設值即命名權）。
