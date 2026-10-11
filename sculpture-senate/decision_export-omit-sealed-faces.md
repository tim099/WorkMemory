---
id: decision_export-omit-sealed-faces
topic: sculpture-senate
title: export obj 省略封閉面只管匯出、不取代補空腔
type: decision
status: active
created_at: 2026-10-12
created_by: unknown
links: []
related_docs: []
---

export obj 預設省略「朝向封閉空腔的面」（TASK-0498）：空腔定義只有一份 —— SCP_SculptAnalysis.cs 的 AirMap（外框外擴一格、從角落 6 連通灌外面的空氣），stats structure 與 export 共用。
判的是這次匯出的那批格（region／exclude_color／mark／links 濾過之後）；sealed=keep 照常輸出；外框超過 MaxStructureCells ⇒ sealed=skipped 照常輸出並說原因。
⚠ 這只管匯出，⛔ 不取代作品資料層的補空腔（Tim 2026-10-11：之後做 voxel 海戰，空腔被打破後要看得到；補空腔照做、已補的保留）。
憲法號讀數：mark=補空腔:begin 照新規則匯出的 obj ＝ 補完之後那一刻逐字相同（107,225 面）；keep ＝ 舊輸出（120,832）。
