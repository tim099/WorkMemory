---
id: knowhow_atlantic-island-town-recipe
topic: town-replica-from-plans
title: 大西洋的島蓋城：自己一區檔、泡水平帶寫 floor、bathy 索引只改那幾格、數船用 rep.fleet.trade
type: knowhow
status: active
created_at: 2026-11-03
created_by: unknown
links: []
related_docs: []
---

大西洋的島蓋城（TASK-1180 豐沙爾、1181 加拉奇科，2026-11-02～03；照 new-island-town-recipe，多下面幾格）：
1. 範圍開自己一區檔（world/areas/<城>.js ＋ index.js 一個名字，同里斯本）⇒ 合 main 只撞 index.js 一行。手寫那一筆的大括號收在 range 那一行（`]]}`），town_dock、town_districts 再往後接。
2. 岸是 TASK-1089 照 OSM 修過的（tools/atlantic_shore.py）；現代港區的寬填地沒挖。town_check 的「棧橋根部、碼頭線接不上」不是 0 ⇒ 先懷疑泡水平帶：那一島在 ISLES 加 floor 0.3（加拉奇科 28 m ⇒ 0）。
   - atlantic_shore.py --write 會把那一島的筆接到表尾 ⇒ 照 HEAD 的順序放回，diff 才只剩 floor。
   - floor 不動水深圖 ⇒ ⛔ 不整份 --apply；world/bathy/index.js 照 bathy_extract.py 同一套只改 coast 那幾格、edits 雜湊、editsHash（方法見 memory／提交 ada7c4cb）；每次合 main 都取 main 的索引再重做一次。
3. 取樣很慢：tier 4 一次取樣 16～24 分鐘（3db4a031 之後挖岸的岸坡一列一列算，降到 20 秒級）；town_plan --twice、town_check、town_report、town_dock 各自取樣 ⇒ 串在背景、一支一支跑。
4. 數「停靠的船」：無頭模擬的船隊走粗略層，不叫 fleetArrive；用 townDay 回傳的 rep.fleet.trade 數在這座城買賣過的船隊（先在別的城驗這把尺量得到）。
5. 史料找不到舊港灣的圖（加拉奇科 1706 年被熔岩填平）⇒ 照今天的岸蓋、單上寫查不到、挖不挖給 Tim 選；點位用 OSM Nominatim 查地標（陸門、堡、教堂）。
6. 反向對照用共用 main 烘好的 townplans：抄進 worktree、town_bake --check 看「換了」的只有自己的檔（或別人剛合的城）⇒ --all 重烘逐份比 plan。
