---
id: pitfall_stern-quarter-glass-gap
topic: sculpt-nightlamp
title: 船尾兩舷窗一格厚的斜玻璃漏水（TASK-1042）
type: pitfall
status: active
created_at: 2026-10-28
created_by: unknown
links: []
related_docs: []
---

夜燈號船尾兩舷窗（窗台以上整片一格厚的玻璃、牆往裡收又沿 x 斜）：Blockside 超載極限（75_leakcheck.js）報「從窗（玻璃旁的縫）進水」，再多載 2,436 噸。
- 原因：相鄰兩格玻璃只在邊相碰；有一處 y 一次跳兩格（x 85–86：z 117 在 y 105、z 118 在 y 107）。
- 修法兩刀（design/tools/）：
  - glass_bridge.py：邊／角相碰的補一格。
  - glass_overlap.py：每一柱 (x, z) 的 y 段要跟鄰柱疊到。第一刀擋不住跳兩格的。
  - 都補在窗的內側（左舷 y 大、右舷 y 小）。
- 座標：遊戲漏點的格座標 ＝ 組裝件的座標（子作品 link 都在 0,0,0），不用換算。
- 量法：Blockside 工作區 `admin.py cache` 不夠（_cache 要從 D:/Unity/Blocksail/external/Blockside/VoxelKit/_cache 抄 *.vxk）⇒ `reimport --id nightlamp` ⇒ reports/shipcheck_nightlamp.md；遊戲裡 `?mode=observe&ship=nightlamp&debug=1` 的 `__game.leak(0).list` 列每個漏點的 gate。
- 窗補完最先漏的是「砲門旁（塞住了還漏）」2,598 噸／4.60 m，離船尾 59.9 m。漏點在船殼裡面一格斜的實心旁 ⇒ 多半是檢查塞砲門的框沒蓋滿，Tim 說另外處理（沒開單）。
