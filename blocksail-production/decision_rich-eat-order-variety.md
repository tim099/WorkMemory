---
id: decision_rich-eat-order-variety
topic: blocksail-production
title: TASK-1166 吃飯：錢多的那一層先買（richFirst）、花樣湊不滿就讓回來（varietyFill）
type: decision
status: active
created_at: 2026-11-02
created_by: apex-two
links: []
related_docs: [D:/Unity/Blocksail/docs/PRODUCTION.md, D:/Unity/Blocksail/docs/STORES.md]
---

TASK-1166（Tim 2026-11-01 選 A＋B）：富裕商業吃得比受薪勞動差的兩個病根與修法（world/food.js）。
- A `CONSUME.varietyFill`：一道菜最多佔那一級 variety（0.25）只負責分散；有貨的菜少於 4 道時，第一輪吃不滿的份數回到有貨的那幾道（第二輪不設上限），不掉回主糧。以前 4 人的富裕那池只剩燉蔬菜 ⇒ 家常 1、主糧 3。
- B `CONSUME.richFirst`：townEat 吃飯、加餐照「一人有多少錢」排，錢多的那一層先買（同錢照 strataKeys）；讀數 fs 照舊照 strataKeys 的順序放。以前受薪勞動寫死先吃、把魚、海產、麵包、肉買光。
- 讀數（起始檔、帶船隊）：富裕比受薪吃得差的城日 60 天 739／1020 ⇒ A 125、B 3、A＋B 6；一年 1,290 ⇒ 280。兩個開關都關 ⇒ 跟 main 逐位元組相同（反向對照）。
- 剩下的 280 是富裕真的比較窮：水手躍升進來、托爾圖加第 208 天起 38 人一人 11～13 錢 vs 受薪 37；加上 STRATA merchant 的 spread 20、reserve 30 比受薪存得多 ⇒ 同樣的錢一天能花的只有一半。要不要調 spread／reserve 是另一題（還沒開單）。
- tools/prod_sim.cjs「只有蛋糕一道」那兩條改成驗讓回來、varietyFill 關掉的反向對照照舊最多 0.25。
