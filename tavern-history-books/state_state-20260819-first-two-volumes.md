---
id: state_state-20260819-first-two-volumes
topic: tavern-history-books
title: 第一版工具與前兩冊已 ship；三層指標未 bump
type: state
status: active
created_at: 2026-08-19
created_by: meadow
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Tavern_History_Workflow.md, ucl_core:Docs~/zh-Hant/Workflows/Book_Writing_Workflow.md]
---

## 已完成（2026-08-19，meadow wake#20）

**工具（UCL_Core `f526b93` / `8f03d37`）**
- `Tools~/AgentCommands/tavern_history.py` —— Phase A 三個子命令：`days` / `export-day` / `verify`
- 產物落 `<data_root>/TavernHistory/drafts/<room>/`（`<date>_raw.md` + `<date>_triage.json`），**草稿區不入版控**（Tim 拍板）
- 四種處置：`raw` / `summary` / `appendix` / `drop`（drop 仍上處置總表）

**書籍分類三軸（UCL_Core `8f03d37`）**
- 新檔 `UCL_BooksClassification.cs`（enum + `_series.json` typed model + read-through 推導）
- 新檔 `UCL_BooksShelf.cs`（`op=shelf` / `op=series` / `op=classify`）
- `Cmd_Books` 加三個 op；`UCL_BooksIO` 的 publish 閘與 tip 標籤改看 `origin`

**成書（Books `24167e5` / `805768e`）**
- 第 1 冊 `history-2026-05-16-locks-and-windows`（13 章，紀傳體）
- 第 2 冊 `history-2026-08-11-cannot-find-is-not-absent`（10 章，舊體例，Tim 拍板不重編）
- 系列 `tavern-history`，冊次**按發生日期**不按編纂順序

## 還開著的

1. **三層指標全沒 bump** —— `AgentCommands` 的 Books 指標、`LY` 主專案兩個 submodule 指標都指著舊 hash。
   同事現在 pull 拿不到兩本書與新 Cmd。要 Tim 說 `commit all` 才做。
2. **`source` 欄仍照舊寫出** —— `library.py` 還在讀它，拿掉等於靜默改 wire format。要先改 python 端。
3. **`_root_index` 的「涉及層」欄** —— 我兩支 fragment 都顯示 `—`，還沒查是哪個 frontmatter 欄位餵它。
4. **第 3 本要編哪一天** —— 未定。工具的 `days` 子命令會列可編的日子（目前 69 天可選）。

## 給接手的人：三件不要重踩

- **Phase A 不准過濾**（含公告）。過濾是 Phase B 的事 —— 編者看不到全貌時，漏掉的東西不會喊。
- **附錄章不要寫到跟正文同一個檔名**。我踩過：第 7 章被附錄靜默蓋掉。生之前先列章號↔檔名對照表。
- **`verify` 必須 exit 0 才發表**。「一則都沒漏」沒有憑據的時候，跟「漏了但沒人發現」是同一句話。
