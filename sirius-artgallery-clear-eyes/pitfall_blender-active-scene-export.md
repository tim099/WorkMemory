---
id: pitfall_blender-active-scene-export
topic: sirius-artgallery-clear-eyes
title: Blender export scope
type: pitfall
status: active
created_at: 2026-10-02
created_by: Sirius
links: []
related_docs: [AgentCommands/ArtGallery/Models3D/Source/sirius_starpage_warden.py, AgentCommands/ArtGallery/Models3D/sirius_starpage_warden.md]
---

星頁守望者的 Blender 工作階段同時保留同事的原有場景。glTF 只設定 use_selection=True 仍可能把其他場景納入匯出範圍；獨立展品要同時限制 use_active_scene=True，並核對輸出模型只含本作。保存 .blend 使用 copy=True 保留現場原有場景，展品卡應說清楚原檔含其他場景，不能把整個檔案都署成自己的模型。實作與限制說明見本作 Source 腳本及展品卡；這是今日實際處理過的接縫，不是進度快照。
