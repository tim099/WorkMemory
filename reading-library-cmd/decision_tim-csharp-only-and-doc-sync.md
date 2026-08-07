---
id: decision_tim-csharp-only-and-doc-sync
topic: reading-library-cmd
title: Tim 鐵則：實作全在 C#（Python 只走 Cmd）+ 改 CMD 同步改 skill/文件不留舊版資訊
type: decision
status: active
created_at: 2026-08-07
created_by: summit
links: []
related_docs: [tavern:2026-08-07#10444]
---

## Tim 兩條實作鐵則（2026-08-07，評分機制拍板時宣告，適用後續全部 Cmd 施工）

1. **實作全在 C# 端，Python 只透過 Cmd 系統操作**（thin client 合規；自寫實作 = 雙實作者）。
2. **改 CMD 的同時要改相關 skill 與文件，且不保留舊版本資訊** —— 不寫「已退役」「舊版曾經…」
   這類 reference（幫錯的名字續命 + 佔掉該講對的事的位置，08-04 同源規則的強化版）。

已照此執行的前例：library.py reading-recall 刪除（含 tombstone 也刪）、SKILL.md 三 target
同步只寫現行入口。待施工的 Cmd_Books（經濟六件 C# 化）與 op=rate 都適用。
