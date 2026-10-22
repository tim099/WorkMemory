---
id: decision_room-tier-heel-knobs
topic: blockside-flood-rooms
title: 第 4 檔橫傾旋鈕 wvisc／wputr／wring／wroll 與量法
type: decision
status: active
created_at: 2026-10-23
created_by: unknown
links: []
related_docs: []
---

第 4 檔橫傾與水流的旋鈕（都在網址可調，預設是 Tim 2026-10-23 點過頭的）：
- ?wvisc=50：房間內虛擬管的黏性（1/s，隱式）。量過 2–200 有效、1000 反而少（1.72°）。
- ?wputr=2：放水先放進水口 2 m 內。?wring=3：裝不下半徑加倍（4、8、16 m），⛔ 不再跳去整間最低（船沒斜時 ＝ 中線 ⇒ 一舷進水沒有力矩，這是第 4 檔斜得少的主因，不是「沒縱隔艙」）。
- ?wroll=1.4：第 4 檔水的橫向力矩倍率 —— 手感旋鈕不是物理。憲法號一舷八洞 42.5 秒：原本 1.12° ⇒ 物理改完 2.18° ⇒ 1.4 倍 4.02°（細檔 3.0°）；2 倍 6.43°。
- ?wstream=0：水流照舊畫片（對照用）。
量的方法：?lab=flood…&p_flood=4 讀 rows[-1].fl.roll／comY；分層 zb、分房 zr 兩檔都能比（細檔照水格中心查烘焙的房）。兩檔的水大多在第 1 間（底艙），差在那一間裡的橫向重心。
