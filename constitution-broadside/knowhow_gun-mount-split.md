---
id: knowhow_gun-mount-split
topic: constitution-broadside
title: 砲身與砲座拆兩件：雕刻 split＋雙生 gunmount、遊戲模擬一份畫兩件、pitch
type: knowhow
status: active
created_at: 2026-10-23
created_by: unknown
links: []
related_docs: []
---

砲身與砲座拆兩件（TASK-0836，2026-10-23；造船規格 §4.2.1、§4.2.2）。
- 雕刻端：砲那件子作品 `work sub=split colors=<座的色號> twin="{name}座" twin_meta='{"kind":"gunmount"}'`（不限作者、引擎逐格證明外觀不變、全庫的擺放自動補雙生）⇒ 批次 meta 給每個座 mount_of=<砲的 id>、id=gunmount-<同號>（scratchpad 的 mk_batch.py 照「X」配「X座」生批次檔）。松島號、定遠號的砲生成程式都是 GUN_BLACK 36／GUN_IRON 73／BRASS 180 ⇒ 座 ＝ 73（Armstrong 再加盾 146、手輪 180）。共用元件（37 mm）拆一次會補到兩艘 ⇒ 兩艘都要重匯。
- 遊戲端：05_hull.js gmtPair（decoSplit 之後）—— 模擬網格 ＝ 砲身＋座接一份（原點取兩件 origin 的最小值）、rmesh ＝ 砲身、roff；座是 deco＋always。10_ship.js 把 rmesh／roff／vpre 抄到 sh.models 的物件、座照 gmod 找砲。60_rbatch.js rbatShip 與 rbatCheck 兩處都要改。62_ports.js gelAnim：pitch 只寫 m.vpre。
- 驗法：10 組對拍＋松島號、定遠號互打兩場（scratchpad t_cs/mz.py）要逐位元組相同 —— 這兩場是抓「模擬網格不等於原本」的那把尺；匯入檢查「砲身與砲座」一行；探針 __game.run(3)+tick(3)（無頭的 performance.now 凍住，live() 推不動模擬）。
- 大口徑砲拆座之後退的距離看得出來：Canet 32 cm 預設 38 格穿出砲塔 ⇒ 預設 meta recoil=10。
