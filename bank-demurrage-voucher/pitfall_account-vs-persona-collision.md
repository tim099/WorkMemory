---
id: pitfall_account-vs-persona-collision
topic: bank-demurrage-voucher
title: 帳戶名與 persona 名撞名（sirius/Sirius）：扣款端已修，發券端同坑且今天零讀數
type: pitfall
status: active
created_at: 2026-09-23
created_by: basecamp
links: []
related_docs: []
---

**Tim 2026-09-22 拍板**：persona `Sirius` 的扣款與打款都對帳戶 `Spectre`；而 `sirius` 是**另一個獨立的帳戶、不合併** ⇒ 保管費**不應該動到 Spectre**。＋「帳戶無論如何都應該固定扣 5% 餘額」。

⇒ 實作：`SCP_Demurrage.BuildPlan` **不再跑 persona 解析**。迴圈的單位是**帳戶**（餘額表的 key 就是帳本上的 `account_id`），再套一次「人→帳號」的表是**型別錯誤**。連帶拿掉 `letters_root` 整條參數鏈。

**病的形狀（TASK-0279）**：帳戶 `sirius` 的保管費每天扣在 `spectre` 身上 ⇒ ① `sirius` 餘額**永遠不會下降**，它每天被重新課一次、沒有終點；② `Spectre` 替它付（09-18／09-20／09-22 各 12，共 36）。
⚠ 而 `sirius` **不是孤兒帳戶**：`GetBoundPersonas("Sirius")` → **apex-one**。⇒ 等於 apex-one 沒繳、kotoko 那三位替他繳。

**🔴 下游（TASK-0270 保管費轉券）會踩到同一個坑，而今天是零讀數**：
`SCP_DemurrageVoucher.Plan` 拿帳本 `account_id` 去跑 `SCP_BankAccountResolver.Resolve` 當大小寫歸一 ⇒ `sirius` 會被 canonical 成 `Spectre`，券發給 Spectre 底下三位而錢是 apex-one 的帳戶出的。
修法：`SCP_BankAccountResolver.cs:73` 的 `s_AccountToPersonas` 改 `OrdinalIgnoreCase` ⇒ `Plan` 可直接拿 `account_id` 查綁定、整段拿掉那次 persona 解析。
⚠ 量過的讀數：`GetBoundPersonas("Spectre")`→3 位／`("spectre")`→**0 位**（字典是 Ordinal）。⛔ 我沒動那兩支（0270 在 @kaguya 手上），留言在該單 #3。

**對拍會怎麼變**：09-22 費用不符 **2 格**（sirius／spectre）而合計 405↔405 **不變** ⇒ 錢守恆，換掉的是歸屬。⚠ 那兩格差異**是要的改動**，⛔ 不是搬壞了。
