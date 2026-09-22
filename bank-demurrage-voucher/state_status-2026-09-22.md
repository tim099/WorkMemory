---
id: state_status-2026-09-22
topic: bank-demurrage-voucher
title: 現況與還沒有的讀數（2026-09-22 收工）
type: state
status: active
created_at: 2026-09-23
created_by: kaguya
links: []
related_docs: []
---

**現況（2026-09-22 23:5x）**：①②③④⑤ 全部落地，`TASK-0270` 已結。

- **③④ 串在 Senate**（Tim：「能在 Senate 做的不要放 Unity」）：`Cmd_Demurrage` 在 `SCP_Demurrage.Apply`
  之後呼叫 `SCP_DemurrageVoucher.RunForDate`；報告與**同一則**廣播多一段 `🎟 保管費轉券`
  ＋四個讀數欄（`voucher_type/voucher_ratio/voucher_enabled/voucher_problems`）。`Senate 277c1fe`。
  ⚠ `letters_root` 參數是**被加回來的**（`dd9817e` 才剛移除）—— 舊理由（persona 歸一）今天仍然成立，
  它回來是為了另一件事：券一人一本，住 `letters/<persona>/`。註解裡寫了，別再刪。
- **第一次實發**（Tim 授權）：`2026-09-22` 那批 **405 Token → 405 張 BTC 券**，`issued_rows=8`、`problems=0`。
  逐檔加總 **404.99999999** ＋ myth 那筆除不盡的 **1/1e8 未發** ＝ 405，逐位守恆。
  冪等：再跑一次 8 筆全「跳過」、`issued_rows=0`。金流沒動（fee entries 仍 8 筆／405）。
  ⛔ **刻意不走 `demurrage op=run`**：當天扣繳 12:21 已落盤，那條會再送一次扣款請求
  （冪等會擋，但「擋住了」與「沒送」在帳上同形）⇒ 走只鑄券、不碰錢的 `demurrage-voucher op=issue`。
- **⑤ 反向對照**：三棵拋棄式樹（各複製一份帳本只改設定，零寫入）——
  缺兩格 ⇒ `BTC×1`／`ratio_per_token:0` ⇒ 0 張／`voucher_type:""` ⇒ 0 張；三者 `fee_rows` 都是 8，
  與改動前逐列相同 ⇒ 金流逐字不變。

**還沒有的讀數（⛔ 不是「沒問題」）**
- `./check.sh` **出廠驗收沒跑**（只跑了 `build.sh`）。
- 跨日那一趟**真扣＋自動發券**的完整路徑要等下一個 UTC 跨日才第一次跑到。
- 🔴 歷史錯帳：`2026-09-22` 有一筆 `account_id=spectre` 而 `ref=overnight-fee-2026-09-22-**sirius**`
  （TASK-0279 修好之前的行為）⇒ 那 12 張券發給 Spectre 底下三位。券與錢同向、本趟不必補救，
  但那筆扣繳本身要不要退給 apex-one／Sirius 的帳戶**未判**（球在 Tim／basecamp）。
