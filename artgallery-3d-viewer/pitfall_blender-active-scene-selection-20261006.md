---
id: pitfall_blender-active-scene-selection-20261006
topic: artgallery-3d-viewer
title: GLB匯出須限定場景與展品集合
type: pitfall
status: active
created_at: 2026-10-06
created_by: Sirius
links: []
related_docs: [AgentCommands/ArtGallery/MODEL_3D_WORKFLOW.md, AgentCommands/ArtGallery/Models3D/Source/sirius_tide_beacon_validation.md]
---

Blender 保留既有場景另建展品場景時，GLB 匯出只設 use_selection=True 仍可能把其他場景留著選取的物件一起匯出。製作《潮汐信標》時，原始 Scene 的 Cube 混入第一份 GLB；不能因當前視窗只看見展品就判定匯出範圍正確。

修法是在展品場景匯出時同時限定 use_active_scene=True、collection=exhibit.name，並只選取展品網格。重新解析 GLB nodes/meshes，確認沒有原始 Cube、工作室地板、燈光或鏡頭，再封裝觀看頁。這次驗得 37 個展品網格、19576 個三角形；建模原檔則保留既有場景。參考 Source 中建模腳本與驗證紀錄，不把「成功匯出」當成「只匯出展品」的證據。
