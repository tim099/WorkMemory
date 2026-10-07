---
id: pointer_leftover-tails-closed
topic: reading-senate-migration
title: 0405 之後的 Senate 側尾巴（已由 8f2cf9c 收掉）
type: pointer
status: active
created_at: 2026-10-07
created_by: kotoko
links: [reading-senate-migration/pointer_leftover-tails]
related_docs: []
---

0405 之後的 Senate 側尾巴：**已全數收掉**，不必再補。

- SCP_Core `8f2cf9c`（Tim／meadow，2026-10-05，Refs TASK-405）一次修掉四處：`SCP_Cmd_Book.cs` 使用者可見的舊指引（`Cmd_Library.media_init`、`UCL_BookEditPage`）、`Docs~/Spending/Items/book-donation.md`、`book-tip.md`（改 `senate cmd book --arg bank=`、`SCP_BooksOps.TipCanvasRate`）。
- 讀數（kotoko，2026-10-07，SCP_Core HEAD `53d8d4b`）：`grep Cmd_Library|media_init 的舊指引|UCL_BookEditPage` 在 `SCP_Cmd_Book.cs` 只剩註解裡的 `senate cmd library --arg op=media_init`（現行指令，正確）；兩份 Spending 文件搜 `ucmd run Books|UCL_BooksIO|--arg agent=` 零命中。
- 殘留一行**註解**（非使用者可見）：`SCP_Cmd_Book.cs:219`「Editor 裝 `UCL_BooksGateway`」—— LY/Senate 的 .cs 已無 `UCL_BooksGateway`，只剩 `Senate.Cli/Program.cs:154` 裝 `SenateBooksGateway`。下次有人動這支檔順手改，不值得單開。
