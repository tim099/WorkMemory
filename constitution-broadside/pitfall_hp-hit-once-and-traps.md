---
id: pitfall_hp-hit-once-and-traps
topic: constitution-broadside
title: 耐久／血量的坑（同一顆彈只算一次、長砲門檻不能壓、爆裂彈直擊重複、預設值對拍法、立繪去背參數）
type: pitfall
status: active
created_at: 2026-10-12
created_by: ame
links: [constitution-broadside/state_wip-2026-10-12k]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-12 晚上做耐久／血量時踩到、或審查抓到的：
- **「同一顆砲彈對同一個目標只算一次」**：砲彈一步分好幾個小步，穿過模型外框或帆要好幾步 ⇒ 每步都扣的話，耐久 3 一發就沒。模型用 `b.hitM`（並傳進 `hitModel` 的 skip，免得蓋住後面那一門）、帆用 `b.sails`（記帆物件本身，⛔ 只記編號 ⇒ 兩艘的同編號帆會互相擋）。耐久 1 時第一下就死，所以以前看不出來。
- **長砲（格子）的門檻不能跟著血量往下壓**：外框約一半是船殼木板，門檻一低，砲身打爛也到不了 ⇒ 門檻固定 0.55，血量只管模型砲。
- **爆裂彈直擊**：直擊扣一次、爆風又扣一次 ⇒ 爆風略過直擊那門。
- **預設值不改變戰局的驗法**：改之前先錄兩個情境（`__game.start('eve'); run(40)`、沙盒鏈彈＋爆裂彈打桅 10 發）的 stats＋cloth 字串，改完同種子逐字比。比較式照舊寫（`cnt >= orig * thr`，⛔ 改成 `cnt/orig >= thr` 邊界浮點會差）。
- **HUD 每 6 次才重畫**：量 HUD 內容前先多跑幾步（`run(1/60)` × 7）。
- 白底立繪去背：碰到邊的「亮而中性」（min ≥ 236、spread ≤ 12）＝背景；被圍住的空隙要 min ≥ 249、spread ≤ 3、≥ 300 px 才算；再清 3 輪貼著背景的淺色毛邊。奶油色襯衫 spread ≈ 31 不會中。
