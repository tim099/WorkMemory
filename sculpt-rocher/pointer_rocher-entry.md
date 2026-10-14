---
id: pointer_rocher-entry
topic: sculpt-rocher
title: 岩堡港：正本在作品 notes／todo；產生程式與下一步
type: pointer
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

岩堡港（TASK-0580）正本在作品：Sculpture/works/summit-rocher-01m/notes.md（拍板、史實讀數、房屋讀數）與 todo.md（下一步）。
- 拍板：港名岩堡港 Port-du-Rocher（Fort de Rocher 的 9 m 岩）；電影只取氛圍（片場搭景，沒有實拍城鎮可對）；0.1 m／格；Tim 2026-10-14 看過房屋「ok」。
- 產生程式：design/houses.py（通用房屋：牆種 plaster／timber／stone／plank、屋頂 gable／hip／shed、迴廊、煙囪、吊招牌、吊貨梁）、draft_houses.py、apply_houses.py（create／stamp 差異落格／check 只讀匯出）、draft_site.py（平面圖＋site_height.npy）。
- 房屋子作品標 sub,house；地形那件標 sub,town（觀測頁類型篩選，TASK-0582）。
- 下一步：海面從 z 10 抬到 ≥ 50（作品已設 sea.json z 60），平面圖整張 +50 重產，再鋪地形。
