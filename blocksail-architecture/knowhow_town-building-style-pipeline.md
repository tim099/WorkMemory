---
id: knowhow_town-building-style-pipeline
topic: blocksail-architecture
title: 新風格的城鎮建築怎麼產、怎麼驗（es 那一套踩過的六個坑）
type: knowhow
status: active
created_at: 2026-10-26
created_by: basecamp
links: []
related_docs: []
---

做一套新風格的城鎮建築（TASK-0975，basecamp-es-01m，2026-10-26）照這條線走，踩過的坑在後面。

- 產線：作品 design/ 抄 basecamp-livestock-01m 的 build.py／apply.py／diag.py ⇒ build.py（產 masks、manifest、verify、圖紙）⇒ apply.py create／stamp／anchors／links／models／sails／check ⇒ Blocksail worktree：tools/townmodels.py MODELS 加一行 ⇒ townmodels.py export <名…> ⇒ build（約 8 分鐘）⇒ townfolk.py。
- 驗：tools/town_foot.cjs 逐塊比 main 與分支挑到的模型（worktree 沒有 Blockside 子模組 ⇒ 抄一份、把 ROOT 換成 worktree、63_towns.js 換成共用那一份的路徑）；town_bake --check。

坑：
1. build.py 一次產好幾件時，其中一件丟例外 ⇒ manifest、verify 整份都沒寫，前面跑完的幾件也不在清單裡；之後 apply 會靜默跳過它們。⇒ 例外修好後，把前面那幾件也點名重產一次。
2. 牆的 helper（stucco_walls 一類）會清掉牆裡面的格子 ⇒ 地板要在牆之後鋪，不然上面的東西浮空。
3. 借來的 helper 有自己的圖層（english.balcony 預設 gallery）⇒ 那一棟的 layers 沒列 gallery 就不會上傳，引擎比對會缺格。
4. 木格柵、欄杆用方格，不要用斜格：斜格只有角碰角，6 連通算成浮空。
5. townfolk.py 重寫 index.js 的 BSTOWNKIT.folks 時，會排到 fauna 那一行後面（內容相同）⇒ 提交前挪回原位，diff 才只有新模型。
6. 在城鎮模式拍照：G.town([x, z]) 的地面高度在 .at.ground。給 G.cam 的 tgt 只要有一個 null，鏡頭就壞了，之後設什麼都救不回來 ⇒ 只能重載地球儀、重新進城。
