---
id: pitfall_yards-nav-magazine
topic: sculpt-punt
title: 上下方帆桁一起轉、平台模型切碎走路格、沒砲也要彈藥庫
type: pitfall
status: active
created_at: 2026-10-20
created_by: unknown
links: []
related_docs: []
---

蓬特號（TASK-0754）踩到、之後的船也會撞到的三格：

1. **上下兩根方帆桁要一起轉** ⇒ 上帆桁 `yard=course`、下帆桁 `yard=lower`（任何不是 gaff／boom 的值）。遊戲 30_rig_physics.js braceYards：非 gaff／boom 的帆桁一律轉到同一個角度；寫成 `boom` 會照縱帆擺，兩根分開、帆被扯歪。findMasts 只拿 `yard=course` 那根的 pivot 找桅。
2. **平台上的模型會把走路格切碎** ⇒ 42_hands.js：走路格避開砲（外框外擴 1 格）與舵（照站的那一段佔的格），而且只留「最大那一片」、小於 12 點（NAV_MINREG）的整塊丟掉。艉台上兩把弓的外框加上兩支舵柄末端，把梯頭那一塊夾成不到 12 點 ⇒ 梯子被尋路丟掉（匯入檢查「尋路沒收這一座：上端接不上那一層的走路格」）。修法：模型讓開梯頭那一帶、迴旋角收小。
3. **沒有砲的船匯入檢查會 ✗「沒有彈藥庫」**；有槳就要 `cfg.base`、陸戰隊與軍官要 `geo.spar`／`geo.marines`（沒填吃憲法號的數字，全員站不下）。

指路：產生程式與讀數 `Sculpture/works/apex-one-punt-01m/design/`（draft_*.py、verify_*.json）、筆記 notes.md；換皮大砲「弓」元件 `apex-one-punt-01m-gun-bow`、舷側舵槳 `-rudder`、石錨 `-anchor-stone` 都是通用部件。
