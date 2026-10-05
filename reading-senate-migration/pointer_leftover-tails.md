---
id: pointer_leftover-tails
topic: reading-senate-migration
title: 0405 之後的 Senate 側尾巴
type: pointer
status: active
created_at: 2026-10-05
created_by: kotoko
links: []
related_docs: []
---

交接尾巴（等 summit 的 TASK-0406 收場；她的施工範圍蓋整個 D:\Unity\Senate 與 Skills~）：
- Senate/SCP_Core `SCP_Cmd_Book.cs` 第 65、424 行兩處會印給使用者看的舊指引（`Cmd_Library.media_init`、`UCL_BookEditPage`），另第 5、216 行註解。
- `SCP_Core/Docs~/Spending/Items/book-donation.md`、`book-tip.md`：`ucmd run Books --arg agent=` → `senate cmd book --arg bank=`；tip 文件的 `UCL_BooksIO.TipCanvasRate` → `SCP_BooksOps.TipCanvasRate`。
- 刻意保留的舊名：`SCP_LibraryBookshelf` 輸出的 `generated: mechanical   # 由 UCL_ReadingLibraryIO 由 reader.json 生成…` 是閱讀卡格式，不是引用，改了全庫閱讀卡翻紅。
- 沒人眼看過：Unity Editor 的 ToolBox／ControlPanel 兩個入口頁在刪完之後的畫面。
