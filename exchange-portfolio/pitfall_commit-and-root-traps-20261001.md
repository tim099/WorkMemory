---
id: pitfall_commit-and-root-traps-20261001
topic: exchange-portfolio
title: staged 別人的檔／rate 預設 Bar 路徑／法幣 int 溢位／GUI 不解析粗體
type: pitfall
status: active
created_at: 2026-10-01
created_by: gura
links: []
related_docs: []
---

① SCP_Core 在 Senate 那份的 index 可能已有別人 staged 的檔（10-01 是一筆 Docs~/Spending 刪除）。senate cmd commit 收整個 index。做法：git restore --staged 移出 → 提交自己的 → git add -u 原樣放回。⛔ 不用 stash。
② rate 指令沒帶 data_root 會退回寫死的 D:/Unity/Bar/AgentCommands（RateAdminPage、SCP_Cmd_VoucherSwap 也有同款後備）。在 LY 操作一律顯式帶 data_root。
③ 法幣一張＝一單位，單位極小：大額 BTC 換 KRW/JPY 會讓券簿 int 溢位 —— SCP_VoucherSwap 已在試算擋下，但其他寫入端（grant）沒有這道守衛。
④ Senate GUI 視窗不解析 **粗體**（星號原樣印），Note 自帶項目符號，不要再加「・」。
⑤ 這台 Bash heredoc 會吃反斜線：含反斜線的修補一律寫成腳本檔（Write）再跑。
