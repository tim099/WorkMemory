---
id: pointer_consume-needs-prosp-1108
topic: blocksail-production
title: 需要表、繁榮度（TASK-1108）的開關、讀數與開著的
type: pointer
status: active
created_at: 2026-10-30
created_by: apex-one
links: []
related_docs: []
---

TASK-1108 合進 Blocksail main 60dda3bc（2026-10-29）：三個開關，預設開，數字全是佔位；每個都能單獨關回舊行為。
- `CONSUME.needs`（world/food.js）：需要表。need 層（柴、鹽、布、肥皂、醋、陶器／銅器）從整個錢包扣今天主糧之後買；better 層（紙、書、寢具、鞋、鐵件、工具、菸斗、藥、木桶）用餘錢買。同一類照 s ∝ w×(價×r)^−σ 分份額（σ=3，max 夾住）；每一樣各自飽和 ⇒ 種類越多總消費越大。surplus：價格低於 base×thr、存貨超過 keep 天的部分才買（非剛需，原料優先給生產鏈）。關：`CONSUME.needs = null`。
- `TOWN_LEVEL.prosp`（world/food.js）：繁榮度＝供得上的貨種類，180 天平滑；加生產力（EFF.a，所有生產建築）與工資（EFF.wage，‰）。關：`TOWN_LEVEL.prosp = null`。
- `COTTAGE.on`（world/townstate.js）：家庭小作坊，見 blocksail-labor-absorb 那條。
- 文件：docs/GOODS_BALANCE_GAMES.md §5（機制與現實依據、四年讀數、開著的）。
- 坑：反向對照（三個全關 ⇒ 城鎮帳雜湊跟 main 相同）合 main 前要對「新 main」重量；prod_sim consume 的舊測試要在 needs=null 下跑。
- 開著：鐵鏈卡（鐵件、工具、木桶 97～99% 卡熟鐵／木炭／鐵礦）；人口比 baseline 少約 2% 沒查；chain_check、auto_check、labor_need 沒照新消費重對。
