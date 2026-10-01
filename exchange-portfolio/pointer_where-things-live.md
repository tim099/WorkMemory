---
id: pointer_where-things-live
topic: exchange-portfolio
title: 文件與程式落點
type: pointer
status: active
created_at: 2026-10-01
created_by: gura
links: []
related_docs: [ucl_core:../SCP_Core/Docs~/Portfolio.md]
---

規則文件：SCP_Core Docs~/Portfolio.md（cmds: [portfolio]）。程式：SCP_Core Runtime/Market/SCP_Portfolio.cs、Runtime/Cmd/SCP_Cmd_Portfolio.cs、SCP_RateSource.cs（fx_rates_per_usd）；掛點 SCP_VoucherSwap、SCP_DemurrageVoucher.Issue、Senate.Core/Cmd_Voucher（grant/consume/migrate）。頁面 Senate.Cli/Pages/PortfolioPage.cs。selftest 群組 market（SelfTest.Portfolio0371.cs）。
