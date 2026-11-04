---
id: pitfall_angra-plateau-relief-bake-order
topic: blocksail-europe-ports
title: 新城：台地補局部等高線、dock／城區先寫再全部重烘、快轉不叫 district_reach
type: pitfall
status: active
created_at: 2026-11-03
created_by: apex-two
links: []
related_docs: []
---

TASK-1179 安格拉（2026-11-02/03，apex-two）蓋新城時撞到的三件：

1. **城坐在 50 m 線下面的台地上 ⇒ 棧橋長不出來**：世界地圖最低一圈等高線 50 m，安格拉整片 40～48 m、離岸 70～100 m 就掉到 0 m（> 18°）⇒ 生成器「市中心走不到港口旁的岸」、棧橋 0 座、全部錨地。量法：BSW.terrain 印 4 m 格的高度與底質（暫存區 terr.cjs 的做法）。修法：tools/relief_rings.py ZONES 加一框（要 z10 terrarium 圖磚，在共用 _cache/terrain-raw/tiles10；下載要 Tim 點頭）⇒ --write ⇒ bathy_check。合 main 撞到 world/bathy/index.js ⇒ 取 main 那份、重跑 --only <框> --write。
2. **順序**：town_dock、town_districts 的 --write 會改範圍檔，指紋照「整個檔」算 ⇒ 全部的城一起過期。先寫 dock＋城區 ⇒ 最後才 town_bake --all（沒有取樣快取約 70 分鐘；同時是反向對照，也是 land_start 的前提）⇒ nav_ports（讀烘好的規劃，沒烘的城 pier 會變 none）⇒ nav_net ⇒ nav_bake。反向對照的基準雜湊：共用工作區 world/townplans/_bake.log 的「烘好 … 雜湊」。
3. **快轉合進 main 不會叫 pre-commit**：聖喬治快轉進 main 時一區的城離中心 2,084 m（外海 dock-titan），district_reach 量的是全部的城 ⇒ 下一個動範圍層的提交才被擋。切法：外海的 dock-titan 給一個靠它那頭、在陸上的種子一區。

另：新城加進尋路圖會改別人的長邊（安格拉讓聖喬治 ↔ 君士坦丁堡、豐沙爾、里斯本換成經它的兩段）⇒ 提交訊息、酒館要寫。
