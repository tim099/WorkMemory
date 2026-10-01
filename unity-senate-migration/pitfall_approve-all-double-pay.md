---
id: pitfall_approve-all-double-pay
topic: unity-senate-migration
title: 一鍵批准全部重複付款：兩條各自冪等的路合起來不冪等
type: pitfall
status: active
created_at: 2026-10-01
created_by: basecamp
links: []
related_docs: []
---

**一鍵批准全部第一次真的按就重複付款**（2026-10-01 LY，cc 591929／zeta bc2988 各付兩次，多 113；Tim：不收回）。三格疊起來才會發生：
1. 後台的 `Start()` 同時只收一件背景工作 ⇒ 迴圈逐張 `Start` 只有第一張跑，其餘被「前一筆還沒完」吞掉，畫面卻印「已送出 N 張」。
2. 待審清單在 `Start` 送出當下重讀 ⇒ 背景工作還沒跑完，讀到舊清單 ⇒ 人再按。
3. 增發寫 `payout/<id>`、央行撥款寫 `payout/<id>/out`＋`/in` —— 註解說「共用同一把冪等鍵」，帳本層是三把不同的鍵 ⇒ 跨路擋不到；而 `Decide` 的「不是 pending」檢查排在動錢之後。
修法（Senate 53355c6）：整批一件工作、做完才 MarkStale 刷清單、動錢前 `PayoutApprovalGuard`（回讀 pending ＋ 帳上有沒有 kind=payout_request∧ref=單號的入帳腳，**不看鍵形狀**）。
⇒ 判準：**「每張單只生效一次」要看帳上的事實（這張單有沒有入帳），不要看冪等鍵的字面** —— 兩條寫入路徑各自冪等，合起來不冪等。
