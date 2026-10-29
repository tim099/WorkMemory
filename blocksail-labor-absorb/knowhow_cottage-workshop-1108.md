---
id: knowhow_cottage-workshop-1108
topic: blocksail-labor-absorb
title: 家庭小作坊：失業緩衝的做法與數字（TASK-1108）
type: knowhow
status: active
created_at: 2026-10-30
created_by: apex-one
links: []
related_docs: []
---

家庭小作坊（TASK-1108，Blocksail main 60dda3bc；world/townstate.js `BSW.COTTAGE`／cottageDay）：同採集，城鎮自帶的虛擬崗位，失業緩衝。
- 菜單只收現有配方（不另開貨）：棉布、繩、龍舌蘭繩、肥皂、陶器、捲菸、醃肉／野味／魚、鐵釘、木炭、麵粉、木薯餅、木薯硬餅。鞋、銅器、蜜餞、陶菸斗等 TASK-0992 的配方進來再加。
- 一人產出與原料 ×eff 0.5、工資 ×pay 0.5（城鎮金庫付）；每一道上限＝本地需求 d × cap 0.5 ÷ 一人產量；先篩「一人份原料有、ρ > stop」才分人，否則人分給沒料的配方＝白領錢（第一版的坑）。
- 四年讀數：小作坊 502 ⇒ 51 人、失業 680 ⇒ 122，會自己退場。cap 調 1 時多收的人是從公共工程搶來的（失業只少 59）⇒ 留 0.5。
- auto.js 的 free、strataPay 的城鎮工資、stats.js 對帳都要算進 jobs.cottage／cottage 產出與原料。
- 測試：tools/emi_check.cjs ⑥d（①～⑥c 關著小作坊量）。
