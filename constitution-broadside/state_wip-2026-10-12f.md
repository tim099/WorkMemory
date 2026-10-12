---
id: state_wip-2026-10-12f
topic: constitution-broadside
title: 2026-10-12 晚上：手動航行、兩舷各自開火、彩色砲列、硬度 20 做完（工作區未提交）
type: state
status: superseded
created_at: 2026-10-12
created_by: ame
links: [constitution-broadside/state_wip-2026-10-12e, constitution-broadside/state_wip-2026-10-12g]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-12 晚上的現況（ame）。

**已提交**（ArtGallery）：`6f6055b`（TASK-0513）、`54be17d`（Refs TASK-0514／0515／0516）。父層 Valhalla 沒 bump、沒 push。`VoxelKit/_cache/constitution.vxk` 不入版控。

**工作區、還沒提交**（ArtGallery 5 個檔：`_build/…/game.js`、`template.html`、`README.md`、成品 `.html`、展品卡 `.md`）。讀數在 TASK-0514 留言 #4（降旗）、#5（人手集中、帆腳索、自動航行）、#6（手動航行、兩舷）；TASK-0515 留言 #3（帆腳索）。
- 降旗設定（預設不降旗，除非全滅）、`LIM` 共用上下限、降旗後不開砲；煙 2.5–5.5 秒；帆掉下來是平面布片。
- 人手集中：`reassign` 每層甲板、每一舷各自借最近的人；`leaveGun`／`goToGun`。
- 帆腳索：`s.lines` 帆掉／被帶走就斷 ⇒ 提交帶 `Fixes TASK-0515`。
- 航行：`sail()`（VMAX 3.6、POLAR、帆裝 `sailSet` 0–2、`s.furled`）、自動操舵（`chooseSide` 決定把哪一舷對著敵艦）、手動舵（`KEYS`、`sh.rudder`、Tab／A／D／W／S）、`collide()`、船速繼承、前置量、航跡、鏡頭跟中點。`WIND` 是側風。
- 兩舷：`砲-P` 與 carronade-P 進砲列（53 門）；兩舷各有砲班（CREW_FULL 6／3／4、MIN 3／2／2，每艘 274 人）；`bears()`／`anyBears()`：對得到才開。Tim 明說「不用換舷，之後多艘船兩舷一起打」⇒ ⛔ 不要再做一班顧兩舷。
- HUD：`gunRow(sh, side)` 兩艘都列兩舷，形狀＝裝填、顏色＝人手、▶＝對得到；下方「⚠ 射界外」。
- 船殼硬度 0.5–20、滑桿對數刻度；鏈彈 `slab` 吃硬度。

**還沒做**：頂風失速／換舷／羅盤（提案 4、6）；長砲損壞分級；送藥手影響裝填、站位修正；重量整場；重截縮圖（`?cam=` 固定機位要改成跟著船）；整張展品卡。審查剩：hurt 效能（#7，人多了更重）、crush 方向（#5）、落下的人穿甲板（#19）、舵手沒站位（#21）、替代品貼圖溢格（#14／#26）、mipmap（#29）。
**要記得**：
- 電腦對電腦 60 秒要算 9.6 秒（人多砲多），斷桅那一步最慢（CPU 70–80 ms）。
- 命中率一局之內起伏很大（40–70%），比較兩邊要換種子（`?seed=`）。
- 窗格太窄（約 360 px）時兩個 HUD 面板會疊在一起；量畫面時用 `resize_window` 1280×720，量完要回 desktop。
