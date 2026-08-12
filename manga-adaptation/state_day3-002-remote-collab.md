---
id: state_day3-002-remote-collab
topic: manga-adaptation
title: Day3 — 002 話 4/10 定案；p05 停在斷針 Step 3a；單圖鏈路三拍板入繪師篇
type: state
status: active
created_at: 2026-08-12
created_by: summit
links: [manga-adaptation/state_day2-v3-flow]
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Manga_Adaptation_Workflow.md, ucl_core:Docs~/zh-Hant/Workflows/Manga_Adaptation_Author.md, ucl_core:Docs~/zh-Hant/Workflows/Manga_Adaptation_Artist.md, repo:AgentCommands/ArtGallery/Comic/template/README.md]
---

2026-08-12 summit：《桅頂的賭注》002 話遠端協作日（Tim 出門、@通知自動 ding 模式首戰）。

## 進度
- **002 話 4/10 頁定案**：p02（三輪：英文招牌→和風化→過）、p03（一次過，瞭望手牌首入畫）、
  p04（一次過，凜與 rin_v1 逐項吻合）。p01 前日已過。
- **p05 進行中，停在 Step 3a**：斷針五輪不收斂後改「先出道具設定圖→掛圖逐格單獨生成→機械併版」
  新鏈路；`broken_needle_v1.png` 已過驗回填設定卡；3a（擱針單圖）待 gura 額度冷卻後交。
  ⚠ 接手者注意：**p05 現檔的 panel 1（圓孔軸帽）與 panel 3（X 幽靈桿）仍是錯的**，等 3a/3b 置換。

## 本日拍板（全部已寫進繪師篇, 見 related_docs）
1. **§二-5 複雜元素單獨出圖**：識別規格道具特寫／3+ 硬規格／伏筆兌現格 → 拆單圖再併版。
2. **併版=機械動作**（裁+縮+貼），不是生成動作 —— 過生成器一次=重賭一次。
3. **道具首次入畫前，圖版設定圖必須先畫先驗** —— image_versions 空著就開格=拿文字重建賭。
   血證對照：銅牌有圖一次過 vs 斷針無圖五輪錯。

## 資產新增/修訂（ArtGallery 工作樹, 未 commit）
- Props 新增：chart-shop.md（含 p01 為準條款）、lookout-tag.md（六分儀+浪紋徽記, p03 畫面即定案）
- Characters 新增：father.md（002 只准羅盤背刻痕, 禁人形；刻痕規格指向 bronze-token §三 不重抄）
- broken-needle.md：v1 圖過驗註記；002.md：charon(回憶) 自 frontmatter 移除（本話零畫格）、
  「日式右開き」的「日式」拔除（v2 和風化真兇）、p02-p04 圖已嵌。

## 驗收方法論（本日驗證有效）
- 細節判定前先裁圖放大（crop_review.py 在 summit session scratchpad, 接手者可重寫, 30 行）。
- 對別人的產出先驗後判：本日兩樁「冤案」（耳環=捲髮、頭巾=人設本有）都在下定論前洗清。
- 繪師回報與像素可能不符（v4/v5 兩輪）：驗收只認像素；已給 gura「交件前放大自檢」閘門。

## pending（p05 之後）
- p06-p10 未畫；P8 鉤子頁必按 §二-5 拆（暗紋大特寫+羅盤背刻痕單獨出圖）。
- compass 圖版仍未繪且須與銅牌暗紋同輪定（002-P8④ 逐筆對照）。
- masthead / signal-flare Props 未寫；卡戎（004 前）/ 圖恩後續 / 父親人形（待定）人設未建。
- 004 負面規格：全話不得出現羅盤暗紋斗篷。
