---
id: knowhow_rigging-rests-on
topic: sculpt-matsushima
title: 索具在 apply_rigging.py、繩端綁模型要 ropecheck fix、模型坐在模型上用 rests_on
type: knowhow
status: active
created_at: 2026-10-24
created_by: ridge-001
links: []
related_docs: [senate:Docs/Workflows/Sculpture_Shipbuilding.md]
---

- 索具在 design/apply_rigging.py（固定點、24 條繩、重拉繩梯、最後 ropecheck fix=1）；apply_links.py 拉的五條靜索與舊的側支索固定點會被它蓋掉 ⇒ 跑完 apply_links 要接著跑 apply_rigging。
- 繩端寫座標落在模型上（信號桁兩端）⇒ ropecheck fix=1 改綁 model:…，遊戲 ropebind 才不猜；重匯後看「照外框猜 0、不靠 0」。
- 甲板上的落點先驗：腳下是那層甲板的格、上面兩格空（不壓欄杆、梯子、艙口）—— apply_rigging.py deck_spot 會擋。
- 鋼鐵艦的拉索要避：吊艇桿起重索繞過下桅樓（拉到上桅樓外緣底下）；煙囪前拉索停在艦橋撐柱前；斜桁不拉穩索（落點在主砲轉得到的範圍）。
- 模型坐在模型上（舵輪 → 舵輪座）：遊戲只看腳下甲板 ⇒ Blockside 10_ship.js restOn（meta rests_on 或舵輪自動綁外框碰得到的 wheel_stand，2026-10-24）。新的「掛在別的模型上」的東西要比照：要嘛寫 rests_on（規格表那個 kind 要加鍵與 refs），要嘛在 10_ship.js 加自動配對。
