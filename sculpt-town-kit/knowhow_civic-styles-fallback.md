---
id: knowhow_civic-styles-fallback
topic: sculpt-town-kit
title: 公共建築照地區文化挑、加勒比海與英國那一套、擺到城裡（TASK-0772）
type: knowhow
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

TASK-0772（2026-10-21，ame）：公共建築照地區文化挑、加勒比海整套＋英國那一套（皇家港）＋共用的港口配套。作品 Sculpture `ame-carib-civic-01m`；正本 BUILDING_SPEC §6.6、§6.7，TOWN_BUILD §2.1。

**挑模型的退路（只住一個地方）**
- 退路由 `world/areas.js` 的 `BSW.townStyles(style, culture)` 算：城的 style ⇒ culture ⇒ common ⇒ 各自的 fall（common 的 fall 是 fr）。
- 結果寫進規劃的 `styles`。Blockside `townPick`、`world/town_top.js` 照它一個一個找。
- 新增一套 ⇒ `TOWN_STYLES` 加一列、模型 meta 寫 style、`tools/townmodels.py` 的 `MODELS` 加名字。命名：`carib-` 地區、`common-` 共用、`en-` 勢力。

**接著蓋的人**
- 產生程式 `design/` 直接 import summit 那一套的 `design/`（carib.py、houses.py、parts.py；sys.path），不另抄。玻璃窗、煙囪從法式套件借，在 `civic._borrow_fr`。
- 一棟一個函式、`build.py <design> <key>` 只產那一棟。`diag.py <key>` 只讀，印撞格、框外、浮空、門口。
- 落格照 summit 的 apply.py：create → stamp → anchors → links → models → sails → check。

**擺到城裡**
- 特殊建築（`SPECIAL` 表、城鎮那一筆 `special`）可以帶 `as: '<用途>'` ⇒ 報表照它數，生成器不再另擺那一種（Fort de la Roche 算碉堡）。
- 生成的城會替特殊建築接一條路，同碉堡的 fortTrack。
- 手擺的城（皇家港）在產生程式的 `PUBLIC` 用 `atM`（公尺）＋`rot`。
- 挑位置先跑探測：整塊在陸上、離每條路的路邊 ≥ 0.5 m、背面一出去是水。水上的地塊生成器不給高度、遊戲擺不出來。

**驗**
- 挑模型：舊挑法對新挑法逐塊比（`pickcmp.cjs`），外加「公共建築有沒有落到 fr」（`frcheck.cjs`）。兩支都在 ame 的 scratchpad，要用就照 TOWN_BUILD §2.1 的描述重寫。
- 遊戲裡找某一棟：W ＝ `town().center` ＋ (kx·x, y)。center 就是規劃原點換到海域、再減掉海圖原點（`__game.bed().origin`）。
