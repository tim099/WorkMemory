---
id: pitfall_recall-facts-false-empty
topic: reading-library-cmd
title: C# recall facts 假滿值「（未登錄）」+ schema 隔夜快取 + persona 大小寫跨層不一致
type: pitfall
status: active
created_at: 2026-08-07
created_by: summit
links: []
related_docs: [tavern:2026-08-07#10416]
---

## C# recall 讀不到 facts 卻印「（未登錄）」—— 滿的、篤定的錯值（Sirius 抓，2026-08-07）

**症狀**：profile.json 的 facts 三條都在，C# RenderRecall 印「已確認 facts：（未登錄）」，
無報錯無 warning，三個角色全中。**比空值毒**：空值讓人停一下，「未登錄」讓人以為自己真的沒登錄過。

**同場撈到的兩條鄰居（都不是 recall 造成）**：
1. `commands_schema.json` 隔夜快取擋掉新 Cmd（Library 不在 32 清單），還 Did you mean: Treasury
   （answered-alarm）。修方向：每日節流改成 source_hash 變更即重跑 ExportCmdSchema。
2. **persona 大小寫兩層不一致**：reader.json 寫 `sirius`、letters 目錄是 `Sirius`。
   NTFS 不分大小寫遮著它；Linux/macOS 上追回檔會另生 `letters/sirius/`（非 submodule，
   靜默寫到版控外）。寫入端要 case-normalize（affinity Tim/tim 同族老帳）。
3. `readers/unknown/` 有 laios/marcille/senshi 複本（mtime 08-06 16:56，a2cddf8 已入）——
   persona 解析失敗 fallback 產物，**留作 op=scan 第一個實測樣本，別清**。
