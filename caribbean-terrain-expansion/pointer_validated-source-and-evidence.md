---
id: pointer_validated-source-and-evidence
topic: caribbean-terrain-expansion
title: 來源快取與驗證入口
type: pointer
status: active
created_at: 2026-10-23
created_by: Sirius
links: []
related_docs: [D:/Unity/Blocksail/docs/CARIBBEAN_TERRAIN.md]
---

正本為 D:/Unity/Blocksail/docs/CARIBBEAN_TERRAIN.md、tools/caribbean_scope.json；提交 a754b6f 已合回 main。輸入來源清單、原版412檔／新版637檔雜湊、252738點共邊及六組地形塊讀數都在 docs/evidence/caribbean-terrain/；完整取樣以 gzip 保存，不能只拿預覽判定無縫。

可接續的本機來源在 D:/Unity/Valhalla/scratchpad/sirius-caribbean-20261022：raw 有9599張z10、72張z12與兩份NE；baseline_unquantized.pkl 是原始完整來源重抽；expansion_correct/expanded_vectors.pkl 才是保留凹洞到全域聯集的正確向量。expansion_cache 是早期錯誤原型，不能使用。快取必須通過來源、scope、函式與科學套件版本核對；新流程用相容的 tools/terrain_requirements.txt，不能套用本機舊 ContourPy 1.1。

正本合併後再生與再驗證讀數留在 canonical_checks/canonical_rebuild_report.json。閉盆仍受現有外環格式限制，沒有新增水下測深。聖地牙哥與奧薩馬測點先比原版，原錯點兩版均是陸地；港灣內的新點與證據一起保存在 harbour_fixture_check.json，不為測試改資料。
