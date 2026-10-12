---
id: state_wip-2026-10-12c
topic: constitution-broadside
title: 2026-10-12 下午：降旗設定、煙、帆布片做完（工作區未提交）；下一步在哪
type: state
status: superseded
created_at: 2026-10-12
created_by: ame
links: [constitution-broadside/state_wip-2026-10-12b, constitution-broadside/state_wip-2026-10-12d]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-12 下午的現況（ame）。

**已提交**（ArtGallery）：`6f6055b`（TASK-0513 首版）、`54be17d`（Refs TASK-0514／0515／0516：模型、船員、操砲、傷亡、滑桿、三角帆、美術接口）。父層 Valhalla 沒 bump、沒 push。`VoxelKit/_cache/constitution.vxk` 不入版控。

**工作區、還沒提交**（ArtGallery，5 個檔：`_build/…/game.js`、`template.html`、`README.md`、成品 `.html`、展品卡 `.md`）：
- 降旗設定（TASK-0514 第 8 格已勾）：Tim 下午改預設 ⇒ 不降旗，除非全滅。死傷門檻滑桿 10–100%（預設 100% ＝ 全員倒下才停）＋三個勾選（砲全啞 10 秒／桅全倒且死傷 ≥ 33%／軍官全倒且死傷 ≥ 25%，預設不勾）；`?strike=60,guns,masts,officers`。降旗的船不開砲、電腦對降旗或在沉的船停火。
- 滑桿上下限只有一份（`LIM`），網址參數照它夾、讀不懂用預設、滑桿被動過才讀回（審查 #20／#30）。降旗後不能開砲（#23／#32）。
- Tim：煙散快一點 ⇒ 煙壽命 2.5–5.5 秒。帆碎裂會 lag ⇒ 帆掉下來改成 6–16 片平面布片（點精靈類型 4，vT ≥ 16 一律程式畫；fx.png 沒這一列）。
- 量測用的 debug 鉤子：`__game.strike({…})`、`__game.casualty(艘, 比例, 職位)`；`stats()` 多 `struck`／`fate`／`over`／`end`。
- 讀數全在 TASK-0514 留言 #4。

**還沒做**（照優先序）：reassign 人手集中、帆腳索浮空（TASK-0515 等它）、長砲損壞分級、人手史料化與站位修正、重量整場、重截縮圖、整張展品卡改成有船員版（現在只改了降旗那句）。
**要記得**：預設規則下一場約 4–6 分鐘（打到沉）；斷桅那一步是目前量到最慢的一步（CPU 70–80 ms），Tim 如果還覺得卡，先查 integrity／spawnPiece。
