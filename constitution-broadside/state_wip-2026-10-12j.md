---
id: state_wip-2026-10-12j
topic: constitution-broadside
title: 2026-10-12 晚上（wake#13）：物理／拆檔／鏡頭／射界／全收在工作區未提交；下一步模型砲＋血條、彈藥庫殉爆
type: state
status: superseded
created_at: 2026-10-12
created_by: ame
links: [constitution-broadside/state_wip-2026-10-12i, constitution-broadside/state_wip-2026-10-12k]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-12 晚上（wake#13）的現況（ame）。讀數在 TASK-0514 留言 #10。

**工作區，還沒提交**（ArtGallery）：帆與繩的物理、帆的著色、game.js 拆成 `js/` 22 檔（`vxk_inline.py` 新增 `data-bundle`）、預設鏡頭「我艦→敵艦」、瞄準圈（無視深度、跟著敵艦轉）、每門砲自己判定射界（`gunTarget`、追擊砲繞自己的砲軸）、帆裝四段（全收；收著的帆打不壞除非桅斷）。README 與 HTML_GAME_WORKFLOW.md 已同步。
- 拆檔前的 game.js 備份在 session scratchpad（會被清；內容已逐字進 `js/`）。
- `.vxk` 是 18:24 匯的（1,433,632 格）；basecamp 的「長砲改模型」還沒提交（等 Tim 預覽），提交後重匯：砲會從 `--parts` 移到 `rig.models`（`砲-S01..P14`、`chase18-P`）。

**Tim 拍板／喊停**
- 長砲損壞分級喊停 ⇒ 長砲改模型後走「被打到扣血、有血條、歸零飛出去」。
- 物理預設開；關 ＝ 原本的靜態。
- 彈藥庫＋火藥桶（模型）：被打中有機率殉爆，機率選單可設、預設 0%；殉爆威力可調，大到能把船炸斷。basecamp 在問模型要帶哪些 meta（酒館 seq 24095／24096：kind、gun、muzzle／breech／blast 固定點、powder_lb、hp）。

**下一步**：等 basecamp 提交 → 重匯 → 模型砲＋血條 → 彈藥庫殉爆。其餘照驗收清單（頂風失速、送藥手與站位、重量整場、縮圖機位、展品卡）。
