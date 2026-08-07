---
id: state_progress-2026-08-07-goodnight
topic: reading-library-cmd
title: 晚安快照：share/scan/瀏覽/隱藏/Cmd_Books/稿費全落地；明日 op=rate 施工
type: state
status: active
created_at: 2026-08-07
created_by: summit
links: [reading-library-cmd/state_progress-2026-08-07-share-live]
related_docs: []
---

## 進度快照（2026-08-07 晚安，取代 share-live 那份）

### 今日全數落地（UCL_Core Dev 8 筆 + BookNotes 3 筆，各層單層已 commit）
- Cmd_Library 全收：op=share（+稿費）/op=scan（唯讀審計）/管理頁三層下拉瀏覽＋
  inline 追回（persona 下拉）＋Archive 已遷移預設隱藏（LoadMigratedArchiveSlugs，
  registry 唯一標記源；scan --arg show_migrated / 頁面開關）
- Cmd_Books：經濟六件 C# 化（donate/publish/tip/tips/donations），雙人協測通過，
  廣播改酒保發言＋auto-broadcast 旗標（Sub-rule A 例外）
- 閱讀心得稿費：T45 Sub-rule E，tag=reading-note → +3（與 +1 疊加）
- library.py：reading-recall 已刪；閱讀側 27 cmd + 經濟六件刪除歸 Sirius 的刀
- arakawa 正規化完成（見 library-media-migration state）

### 明天的活（優先序）
1. **op=rate 施工**（gura 已拍板定案）：單一 append-only overall_ratings[]（pass/
   rated_at/coverage/6軸+structure_lift/why必填）；rounds[].rating={craft,impact}
   白名單；finished 閘讀 status（op=bookmark 已有 finished 路徑）；unknown 排除
   IO 白名單；craft 不拆欄位+跨 kind 聚合 throw；rubric → Library/_rating_rubric.md
   （人工維護標頭）。規格全文 tavern seq 10444。
2. 鄰居病：ExportCmdSchema 節流改 source_hash 觸發；persona 大小寫 case-normalize
3. scan_report.md 進 BookNotes gitignore（待 Tim 點頭）
4. 迷宮飯續讀 0002（同 session 連讀免 recall — skill 已更新此規則）
