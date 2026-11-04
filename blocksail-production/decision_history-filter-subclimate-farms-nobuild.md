---
id: decision_history-filter-subclimate-farms-nobuild
topic: blocksail-production
title: 照史實篩作物與建築：細氣候區、farms、noBuild
type: decision
status: active
created_at: 2026-11-03
created_by: unknown
links: []
related_docs: []
---

## 照史實篩作物與建築：細氣候區、farms、noBuild（TASK-1178，Tim 2026-11-02～03）

**拍板**
- 史料上有、資料判種不了的作物（百慕達的菸草、玉米：Köppen 1901–1930 是 Cfa ⇒ 溫帶）⇒ ⛔ 不把那一塊改成熱帶（甘蔗、可可會跟著長出來），⛔ 也不把溫帶全開（倫敦、阿姆斯特丹也種得了）。
  Tim：「城鎮(或區域) 可以特別設置氣候細節…而不是直接套用粗略的熱帶」「例如亞熱帶或其他更細的氣候分區」。
  ⇒ 範圍層的 `subclimate`（細氣候區，`world/areas.js` `BSW.CLIMATE_SUBS`，現在只有 `subtropical`）疊在氣候區上面；物種、適地的 climate 清單直接寫細氣候 ⇒ 粗的或細的任一個合就算合。
- 開場的田照史實：城鎮那一筆 `farms: [配方鍵…]` ⇒ 生成器 `townFarms` 只蓋這幾種。
- Tim「也照史實篩」：`farms` 也管自動建築；`noBuild: [建築種類…]` 擋配方不看適地的（鹽田的曬鹽、釀酒坊）。
  兩格由 `tools/land_start.cjs` 抄進開場檔的 `land.farms`、`land.noBuild`；`world/auto.js` `autoRecipesAt` 在三處都照它篩（蓋新的、判能不能換、換成哪一條），開局建造清單走同一支。玩家照舊照適地蓋。

**讀數（聖喬治，起始檔＋發船一年，第 60 天以後）**
- 只有 farms：開局清單有鹽田 3、葡萄園 3、釀酒坊 3；一年下來大麥 21,765、小麥 11,685、鹽 3,213、酒 2,603。
- 加上 noBuild、farms 管自動建築：這幾樣都 0；玉米 53,362、菸草 713。
- 反向對照：別座城的 land 一個位元都不變、規劃逐份相同。

**還沒解**
- 菸草還是少：規劃只放 1 座菸草園（生成器用主糧補田），自動建築照市場挑，多半蓋玉米。要像史實那樣以菸草為大宗，得另外想（例如出口價、或規劃照比例放經濟作物）。
- 鹽田的「曬鹽」配方本身沒有 need.suit ⇒ 每座城都是候選（適地表判「氣候不合」也擋不到）。只有寫 noBuild 的城擋得住；要不要給配方加 suit 是全遊戲的事，沒改。
