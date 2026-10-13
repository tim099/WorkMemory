---
id: pitfall_event-size-split
topic: sculpture-senate
title: 事件檔大小與拆檔：一刀＝頭檔＋續檔；revision 不是內容雜湊；快取是同源證人
type: pitfall
status: active
created_at: 2026-10-13
created_by: basecamp
links: []
related_docs: [Docs/Workflows/Sculpture.md]
---

事件檔大小與拆檔（TASK-0521）——動雕刻事件格式或做大刀之前讀這段。

- 一刀＝頭檔 X.json＋續檔 X_p2.json…；上限 SCP_SculptStore.MaxEventBytes（40 MB，量**寫到磁碟**的位元組，含 CRLF）。認同一刀只看檔名（PartHead／LastPartIndex）。時間點（IndexOf／FindEvent）、history、redo 都照整刀；新加任何「指到某個事件」的地方，記得也走 LastPartIndex，不然會停在半刀上。
- 🩸 revision（SCP_SculptWorks.Revision）不是內容雜湊：它把 LastEventFile 與格子的**列舉順序**也算進去 ⇒ 兩件作品、或快取 vs 從頭重播，內容一樣也不相等。比內容用 SCP_SculptVxk.SameVoxels／Checksum（跟 voxel_export.py／.vxk 同式）。
- 🩸 LoadSpace 讀快取：水位還有效時根本不會重播你剛改的舊事件 ⇒ 拿它驗「改舊事件之後重播對不對」是同源證人。要從頭重播用 SCP_SculptStore.LoadView（不讀不寫快取），舊版 exe 則用 `mark=<最後一個事件>.json` 逼它從頭重播。
- 🩸 事件檔有兩種格式：引擎 RecordEvent 寫 indent=2（每個數字一行），也有單行的（餘燼號 10-12 那幾個）。同一份內容縮排版約大 2.7 倍 ⇒ 重寫舊檔要保留原格式（split-events 會自己判）。
- 舊版 exe 重播得出拆過的刀（只讀 edit.after），但它的 undo 把每一段當一刀。
- 實規模讀數（2026-10-13）：821,491 格變動的 assemble ⇒ 4 檔、最大 38.0 MB。
