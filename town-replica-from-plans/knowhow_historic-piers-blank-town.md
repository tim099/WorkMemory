---
id: knowhow_historic-piers-blank-town
topic: town-replica-from-plans
title: 手擺城的港口：照史料位置擺棧橋＋T 字頭、探針量到 7 m、形狀是遊戲用的
type: knowhow
status: active
created_at: 2026-10-20
created_by: kaguya
links: []
related_docs: []
---

手擺的城（base blank）不生成棧橋 ⇒ 要泊位就照史料手擺 `piers`（主棧橋＋T 字頭，帶 kind／of），泊位由生成器沿著標。

- 先查那座城的港口「長什麼樣」：1453 年金角灣不是港池，是沿海牆的一段段 skala（上岸段，木頭步道伸進水裡、吃水 2 m 的船直接靠岸）；加拉塔沒有防波堤。⇒ 位置照史料，形狀與長度是遊戲用的設計，要在文件裡講明。
- 量法：暫存區 pier_probe.cjs —— 從史料點找最近的岸、72 個方向每 2 m 量水深，挑最短到 7 m 的方向；根從岸上 6 m 起、伸到 7 m 再多 16～34 m、T 字頭兩邊各 40 m。T 字頭外側擺不下大船 ⇒ 主棧橋再伸長，不是改泊位數字。
- 兩岸面對面的棧橋要留水道（金角灣約 490 m，兩邊各 130 m＋泊位還剩約 200 m）；townReach 照舊判通不通。
- 史料沒有的港池（孔托斯卡利昂那種已經填掉的）⇒ 世界地圖 carve（另一條）。當年是槳帆船的港 ⇒ 做成小船港（5 m），大船留在棧橋，不為了泊位數去發明一條沒有史料的深水航道。
