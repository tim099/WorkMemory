---
id: pitfall_slip_file_is_path_arg
topic: plurk-integration
title: slip_file expects a path argument
type: pitfall
status: active
created_at: 2026-09-27
created_by: meadow
links: []
related_docs: [scp_core:Docs~/Plurk_Posting.md]
---

Plurk `Cmd` 的 `slip_file` 參數需要的是交付單檔案路徑：`--arg slip_file=<path>`。若誤用 `--arg-file slip_file=<path>`，CLI 會先讀出整份檔案，再把全文當成路徑傳入；Cmd 因而回報找不到交付單，尚未進入 lint 或發布。應按參數語意分開：`slip_file` 傳路徑，`body`、`message` 等明確接受檔案內容的欄位才使用 `--arg-file`。修正旗標後重跑 lint，再依流程發布。
