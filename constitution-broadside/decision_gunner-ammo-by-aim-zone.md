---
id: decision_gunner-ammo-by-aim-zone
topic: constitution-broadside
title: 砲長照瞄準位置挑彈＋艦長指揮砲長
type: decision
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

砲長挑彈改成照瞄準點的位置（Blockside 8242fb6，51_captain.js gunnerAmmo／aimZone／ammoProps；Tim 2026-10-18）。
- 瞄準點的高度分四帶（照船型 geo：deckAim、rigAim）：水線／砲甲板／甲板／帆具；每一帶看重的屬性 AIM_ZONE_W；彈種只看屬性（AMMO_K 對砲帆人＋AMMO_TAG 打船殼 hull／碰水不會沒 uw／會炸 blast），範圍是 ammoOf(sh) ⇒ 新彈種補一列就會用。
- 用砲長「想瞄的那一點」挑，不是加了散布的（第一輪高度散 ±22 格，瞄砲甲板有一半落到水線帶）。
- 我艦自動開砲寫 AMMO_AUTO，不碰 AMMO_I（玩家選的、記在偏好）；ammoNow() 給按鍵開砲與彈種鈕。
- 艦長→砲長：54_helm.js helmGunOrder（被壓制或敵艦拉開、太遠 ⇒ 帆具；近 ⇒ 甲板人員；戰列 ⇒ 砲甲板；集火 ⇒ 水線），砲長那個模式 × GUN_ORD_K 2.5，命令至少隔 6 秒換。
- ⚠ hull 屬性（1／0.6／0.5）是估的；新舊砲長勝負沒量（要比得加開關）；調參預設是用舊砲長調的。
