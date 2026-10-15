---
id: state_task0603-manual-candidate-20261015
topic: sirius-tidegrave
title: TASK-0603-manual-candidate
type: state
status: active
created_at: 2026-10-15
created_by: Sirius
links: []
related_docs: []
---

Tim 已選擇「我手動驗收」，TASK-0603 維持 in_progress，不宣告完整自動 PASS，也不勾 TASK-0546。

可試玩候選：manual_candidate.vxk，SHA256 9081f22cb8b25c33d65b25420e670e2c6af30571bfadbaaf8805228956f93d80，500 ropes / 166 models。船體與原固定帆點保留；新增實體砲繩與舵輪導環、mizzen兩段brace、主支索桅頂路線、前後支索實木腳座、艏帆原頭環至原帆腳支索及獨立4cm升帆索。所有作品狀態透過 sculpture CLI 寫入。改艏帆sheet導點使自由帆角平衡形狀重算，固定接點沒有搬移。

程式保證固定節點不受碰撞位移，接觸認證要求完整區域、同一真實owner、原材料與當前局部範圍；不以整張帆或繩名豁免。修首次開局誤當展帆與收帆第一材料列。單元檢查通過，preview原檔還原比對通過，實際無頭遊戲shipcheck 0 errors / 9 船舶配置警告（0540範圍，非完整船交付）。

預覽 http://127.0.0.1:8763/index.html?ship=tidegrave,constitution&prefs=0&phx=full&p_sailhit=1 已明確開 rig High + sailhit；正常遊戲預設關閉不變。伺服器需保持運行。候選及來源hash記錄在 D:/Unity/Valhalla/scratchpad/sirius-tidegrave/task0603/manual-preview/MANIFEST.json。

限制：未重跑完整20場景。先前正常1.5ms預算檢查仍有自由帆局部穿透；新的實際候選短測亦須照實保留raw/unresolved，不能把靜態繩路或固定點測試說成完整動態通過。等待Tim手動結果，依具體接觸續修。

完整接手說明 D:/Unity/Valhalla/Sculpture/works/sirius-tidegrave-01m/design/task0603/PROGRESS.md；獨立證據在 scratch task0603/attachment_audit、cloth_review、dynamic_qa。未提交這次修正，請勿暫存其他人同時修改的Blockside檔案。
