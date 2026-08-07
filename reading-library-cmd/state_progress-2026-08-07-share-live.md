---
id: state_progress-2026-08-07-share-live
topic: reading-library-cmd
title: 進度快照：⓪①②收（11c1e9c）+稿費上線；剩③等Sirius過閘、④scan
type: state
status: superseded
created_at: 2026-08-07
created_by: summit
links: [reading-library-cmd/state_progress-2026-08-07-sirius-converged, reading-library-cmd/state_progress-2026-08-07-goodnight]
related_docs: []
---

## 進度快照（2026-08-07 中午，取代 sirius-converged 那份）

### 已收（UCL_Core `11c1e9c`，Tim UI QA 通過）
- ⓪ facts 假滿值修復：讀端 ReadFactsList 吃陣列/字串兩形狀、寫端 FactsToJson 只寫陣列；
  補「作品與媒材」「書架投影」兩節；rounds 容忍 legacy 字串條目。
- ① op=share：registry → Tavern Op_Post 同 pipeline，seq 由 Cmd_Tavern.LastPostSeq
  static slot 遞回 → RecordSharedSeq。同 round 重發被擋。實測 seq 10422。
- 稿費：T45 Sub-rule E，tag=reading-note → +3 token（Tim 拍板，不限媒材），與 +1 疊加。
- ② 管理頁「📖 追回」鈕接 RenderRecall（同一段服務層）+ round 全文開關 + 產生追回檔。
- 文件：Reading_Library_Workflow.md 已同步（share 節 / facts 陣列 / 退位警語）。

### 未做
3. **library.py reading-recall 刪除** —— 閘在 Sirius：dungeon 陣列形狀複驗 + diff 收斂
   （已 @她，等回覆）。過閘前別交錯跑兩版 recall。
4. op=scan / op=migrate —— 第一測資 readers/unknown/（laios/marcille/senshi 複本，別清）。
- 鄰居病待收：ExportCmdSchema 節流改 source_hash 觸發；persona 大小寫 case-normalize。
- UCL_LibraryManagePage.cs 有 Tim 的未 commit 修改（CreateForTitle 接線）—— 他的改動他收。
