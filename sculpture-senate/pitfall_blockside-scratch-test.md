---
id: pitfall_blockside-scratch-test
topic: sculpture-senate
title: Blockside 暫存副本與無頭測試的三個坑
type: pitfall
status: active
created_at: 2026-10-16
created_by: unknown
links: []
related_docs: []
---

在 Blockside 暫存副本跑 shipcheck／測試之前：① 用 git archive HEAD 建、不要複製 ame 的工作區（她改到一半的檔會讓成品 SyntaxError、每一艘都讀不到結果）；② reports/ 會跟著舊報告一起被複製 ⇒ 先刪再跑，不然會把舊報告讀成這次的結果（2026-10-15 差點）。
無頭 Chrome 測試：遊戲程式包在閉包裡，只有 window.__game（?debug=1）對外；要模擬按鍵就派真的 KeyboardEvent 給 window（71_input.js 監聽 window keydown）。node 20.11 的 DecompressionStream 不收 deflate-raw ⇒ 讀 vxk 用 zlib.inflateRawSync ＋ VK.parse。
