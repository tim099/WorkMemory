---
id: state_wip-2026-10-12d
topic: constitution-broadside
title: 2026-10-12 傍晚：降旗、人手集中、帆腳索、自動航行做完（工作區未提交）；手動航行提案等拍板
type: state
status: superseded
created_at: 2026-10-12
created_by: ame
links: [constitution-broadside/state_wip-2026-10-12c, constitution-broadside/state_wip-2026-10-12e]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-12 傍晚的現況（ame）。

**已提交**（ArtGallery）：`6f6055b`（TASK-0513）、`54be17d`（Refs TASK-0514／0515／0516）。父層 Valhalla 沒 bump、沒 push。`VoxelKit/_cache/constitution.vxk` 不入版控。

**工作區、還沒提交**（ArtGallery 5 個檔：`_build/…/game.js`、`template.html`、`README.md`、成品 `.html`、展品卡 `.md`）：
- 降旗設定（預設不降旗，除非全滅）、滑桿上下限共用 `LIM`、降旗後不開砲 —— TASK-0514 留言 #4。
- 煙壽命 2.5–5.5 秒；帆掉下來改成 6–16 片平面布片（點精靈類型 4）。
- 人手集中：`reassign` 每層甲板規劃（借最近的人）、`leaveGun`／`goToGun` 管站位；`c.deck` 用 `gunDeck`（追擊砲算砲甲板）。
- 帆腳索：`s.lines`（名字 `<帆名>.`）帆掉／被帶走就斷；支索斷的三角帆當場掉。修好 TASK-0515 的第二件 ⇒ 提交帶 `Fixes TASK-0515`。
- 自動航行：`sail()`（VMAX 3.6、POLAR、自動操舵讓敵艦在右舷正橫、轉向率 ∝ 航速）、船速繼承、前置量、`inArc`、航跡、鏡頭跟中點、選單「航行：自動／停船」`?sail=off`；`WIND` 改成側風 [0.6, 0, 1.65]。
- 讀數在 TASK-0514 留言 #5、TASK-0515 留言 #2。

**等 Tim 拍板**：手動航行的操作提案（TASK-0514 留言 #5 的六點：Tab 切自動／手動、A/D 舵、W/S 三段帆、頂風真的失速要換舷、左舷也能打（資料裡有 `砲-P` 14 門與 carronade-P）、HUD 風向羅盤與射界指示）。

**還沒做**：長砲損壞分級、人手史料化與站位修正、重量整場、重截縮圖（`?cam=` 固定機位要改成跟著船）、整張展品卡改成有船員與航行的版本。審查剩下的：鏈彈不吃硬度（#15）、hurt 效能（#7）、crush 方向（#5）、落下的人穿甲板（#19）、替代品貼圖溢格（#14／#26）、mipmap（#29）等。
**要記得**：斷桅那一步是目前量到最慢的一步（CPU 70–80 ms）。窗格太窄（約 360 px）時兩個 HUD 面板會疊在一起。
