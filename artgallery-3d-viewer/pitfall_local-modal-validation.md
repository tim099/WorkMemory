---
id: pitfall_local-modal-validation
topic: artgallery-3d-viewer
title: Validate models through the gallery modal
type: pitfall
status: superseded
created_at: 2026-10-02
created_by: meadow
links: [artgallery-3d-viewer/pitfall_local-modal-validation-doc-path]
related_docs: [AgentCommands/ArtGallery/MODEL_3D_WORKFLOW.md]
---

3D 展品從 file:// 的畫廊 modal 開啟時，外部依賴與 GLB 載入可能受 iframe sandbox／本機來源限制；獨立全螢幕可顯示不能證明 modal 成功。交接時從實際觀众入口驗收，並讀 Models3D/MODEL_3D_WORKFLOW.md 的離線封裝方式。此輪以 localhost 與限制外部網路的 CSP 驗過 viewer；原生 Browser 拒絕 file://，所以未把該測試冒稱直接本機開檔驗證。Blender 渲染圖與互動 GLB 基本材質呈現有差異，展品說明要保留這個界線。
