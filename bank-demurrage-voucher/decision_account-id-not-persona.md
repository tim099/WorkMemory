---
id: decision_account-id-not-persona
topic: bank-demurrage-voucher
title: 發券端用帳本 account_id 查綁定，⛔ 不借道 persona 解析
type: decision
status: active
created_at: 2026-09-23
created_by: kaguya
links: []
related_docs: [Assets/Plugins/SCP_Core/Runtime/Bank/SCP_BankAccountResolver.cs, Assets/Plugins/SCP_Core/Runtime/Bank/SCP_DemurrageVoucher.cs]
---

`SCP_DemurrageVoucher.Plan` 要拿**帳本上的 `account_id` 直接查綁定**，⛔ 中間不准插 `SCP_BankAccountResolver.Resolve`。

那支是 **persona → 帳號** 的表：撞名時它換掉的不是拼法，是**換了一個帳戶**。
實測（BTC，2026-09-22）：`Resolve("sirius")` → `Spectre`（persona Sirius 用 Spectre 的帳），
而帳戶 `Sirius` 的主人是 `apex-one` ⇒ 帳本出現 `account_id=sirius` 的扣繳時，
券會發給 Spectre 底下三位，錢卻是 apex-one 的帳戶出的。

**大小寫那半由綁定表自己吃**：`s_AccountToPersonas` 用 `StringComparer.OrdinalIgnoreCase`
（帳本寫小寫、綁定檔存原拼法）。⇒ 歸一要走**帳號自己那條軸**。

落地：`SCP_Core bd6b737`。對拍（Editor 端 `ucmd run Invoke` 打 `UCL_BankResolve.GetBoundPersonas`）：
`sirius`→`[apex-one]`（改前 0 個），其餘 `spectre/luna/myth/zeta/claude-code` 逐位與改前一致，`nobody-xyz`→`[]`。
