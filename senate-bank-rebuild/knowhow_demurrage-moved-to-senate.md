---
id: knowhow_demurrage-moved-to-senate
topic: senate-bank-rebuild
title: 跨日保管費搬進 Senate（TASK-0278）：對拍拿舊帳本產物當基準、三個踩過的坑
type: knowhow
status: active
created_at: 2026-09-23
created_by: basecamp
links: []
related_docs: []
---

TASK-0278：跨日保管費的計算與落帳從 `UCL_BartenderDaemon` 整段搬進 `SCP_Demurrage`（SCP_Core）＋ `senate cmd demurrage`（ServerDelegateCmd）。Unity 只剩三件事：判跨日／派一次／貼廣播。

**廣播為什麼還在 Unity**：`tavern.writer=editor` ⇒ Server 沒資格寫酒館。本文由 Cmd 組好寫進 `body_out` 指定的檔，觸發端讀去貼。⛔ 不在 Server 側偷開第二個酒館寫入端（那是 TASK-0106 在收的線）。

**對拍怎麼做的（TASK-0278 ② 那道閘）**：`SCP_DemurrageParity` 拿舊實作**已經寫在帳本上**的產物跟新實作同快照重算。快照時點＝那一天**第一筆保管費 entry 之前**（⛔ 不是當天 00:00 —— 同一天稍早還有別的金流）。⭐ 這個設計的好處是舊 code 刪掉之後對拍**照樣跑得動**。
讀數：09-20／09-22 費用不符 0、入庫不符 0；09-18 差 1 —— 那天跑的是**更舊的版本**（舊 Treasury 帳本 `b7489f48c` 逐筆寫著 `balance_before=1438` ⇒ 迴圈內即時讀餘額，現行版是迴圈前一次快照）⇒ 不同受測體，不可比。

**三個踩過的坑**：
① **帳號歸一原本發生在寫入端**（`UCL_TreasuryLedger.ResolveAccountOrThrow`）—— 搬進 in-process 之後沒有人做那一跳。
② **對拍的受詞挑錯**：拿未歸一的 id 聚合 ⇒ 八個帳戶全報不符，**而合計一模一樣**；解析器回正式寫法（`Spectre`）而帳本存正規化小寫（`spectre`）⇒ 兩邊都要 `SCP_BankId.Normalize`。
③ **入庫只在「這次真的扣到錢」時才送**：舊實作的入庫金額是回讀餘額算的 ⇒ 同帳戶兩筆扣款被併成一筆 credit、掛在後面那個繳費者的 ref 下（09-20 `credit-…-spectre`＝42＝12＋30，而 `credit-…-sirius` 這把鑰匙**不存在**）⇒ 照送會讓不存在的鑰匙通過冪等檢查、**憑空 credit 進央行**。

**冪等活體**：對已扣過的當天跑 `op=run confirm=1` ⇒ 8 筆全冪等命中、ledger 檔數 18→18、合計 69122 逐位不變。
**反向對照**：費率 0（scratch 設定＋真帳本餘額）⇒ 0 筆 charge。
⛔ **未驗**：新路徑還沒真的收過一次錢 —— 第一次真扣要等下一個 UTC 跨日。
