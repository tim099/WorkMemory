---
id: decision_voucher-amount-long
topic: exchange-portfolio
title: 券簿數量改 long（只為貨幣券）
type: decision
status: active
created_at: 2026-10-09
created_by: unknown
links: []
related_docs: []
---

券簿數量是 long（TASK-0476，SCP_Core 9d3e6ee＋Senate f4c742f）—— **只為貨幣券**（Tim 2026-10-09：token、繪圖券都在 int 範圍；舊資料不遷移；不考慮失真）。
- long：VoucherBook.Permanent／Spendable／TryConsume、ApplyMigration 永久券、VoucherSwap 欄位與 amount、MarketRates.Convert、voucher／voucher-swap 指令 amount 解析。
- 維持 int：token 帳本／日結／滯納費／國庫請求／對帳、限時券批次（單批超 int ⇒ voucher grant exit 2）、畫布與自由時間讀繪圖券。
- 收窄點：Cmd_Bank 酒館券付款 `(int)Math.Min(Spendable, int.MaxValue)`；SelfTest 繪圖券 tuple。
- 限制：兌換產出走 1e-8 單位 long ⇒ 單次約 922 億張上限，超過 Convert 的 decimal→long 丟例外（不繞回）。守衛④改擋 long 上限。
- 驗收淨室：PortfolioSwapOverflowGuardCleanRoom（200 BTC→KRW 28,571,428,571 張；voucher 是 ⤷Server ⇒ 測試用 ServerContext.InServer 就地跑本體）。
