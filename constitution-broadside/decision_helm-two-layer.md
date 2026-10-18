---
id: decision_helm-two-layer
topic: constitution-broadside
title: 艦長開船兩層：艦長只給期望航速、航速調節挑帆與俥鐘
type: decision
status: active
created_at: 2026-10-17
created_by: basecamp
links: []
related_docs: []
---

艦長開船（54_helm.js，Tim 10-17 拍板「艦長指揮時可以直接改為設定期望速度 在由算法去判斷要如何調整帆&俥鐘」）：
· 兩層：艦長（helmThink 每 3 秒／helmSteer 每步）只回航向與期望航速 sh.wantV（Infinity／'cruise'／m/s／負的倒退）；航速調節 helmGovern（每 1 秒）挑帆裝、俥鐘、放風（helmSpill⇒throttle）。新戰術只改 wantV 與航向，⛔ 不要在艦長裡直接撥帆或俥鐘。
· 踩過：反向繞圈時照正橫調速 ⇒ 兩艘一起慢到 1.9 節（只有同向並行才守正橫）；給巡航的數字 ⇒ 轉向後對不上、帆裝來回換（改成 'cruise' 標記）；只有帆的船給負的 ⇒ 收帆停在敵艦船頭（倒退只給有動力的）。
· 不抽 rnd() ⇒ 同一份成品跑兩次逐字相同；改艦長行為 ⇒ 三組對拍基準換新（10-17：cb5fd03668、64e0cee698、420cd3a4ae）。
· 2D 沙盒 sb_sim.js 抄的是舊的交戰操舵 ⇒ 第二期要把開船搬成沙盒也載得進去的共用邏輯（像 53_commander.js）。
