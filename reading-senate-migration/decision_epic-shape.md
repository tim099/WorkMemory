---
id: decision_epic-shape
topic: reading-senate-migration
title: epic 0398 的範圍與拍板
type: decision
status: active
created_at: 2026-10-05
created_by: kotoko
links: []
related_docs: []
---

結構與拍板（2026-10-05，Tim 全包 GO）：
- 閱讀線 `senate cmd library` 早已在 Senate（核心在 SCP_Core `Runtime/Library/`）；本 epic 補的是：`op=share`（組稿＋發酒館＋shared_seq 回執）、`SCP_PathId.ComicRoot`（路徑管理頁「外部漫畫庫根」，舊 `.comic_root.local` 不再被讀，空白而快照有值時 `op=comics` 會明說）、`op=comic_pages`（某話頁檔絕對路徑，內外部皆可）、四個頁。
- 書店線 `senate cmd book` 補 `shelf`／`series`／`classify`／`normalize_donations`（`donations`／`tips` 本來就有，epic 描述曾寫錯）。
- Tim 拍板：酒保定時規則已廢棄 ⇒ `UCL_StringBookRecommendProvider` 刪；`_donation.json` 統一成新版面（2 空格／冒號後有空格／CRLF／結尾換行），用 `op=normalize_donations`（預設 dry-run，confirm=1 才寫）一次轉掉 16 份。
- Unity 端退場於 UCL_Core 0d38e95f（刪 25 檔、改 25 檔）；skill 源頭 `UCL_Core/Skills~`，副本用 `install_skills.py --include` 同步。
- 驗收格式：dev 一格不勾，QA（calli／gura）結單；dev 提交用 Refs。
