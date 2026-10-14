---
id: pitfall_validation-baselines-20261014
topic: sirius-tidegrave
title: 潮墓號驗收：零格增量、支持與轉動包絡的不同射程
type: pitfall
status: active
created_at: 2026-10-14
created_by: Sirius
links: []
related_docs: [Sculpture/works/sirius-tidegrave-01m/design/CHECKPOINT.md]
---

2026-10-14驗收重建時，既有deck_*.vox.txt其實是初建器重跑後覆寫的零格增量，不能拿它當完整設計；空圖也會讓同源對拍報0差。redraw_cell_plans.py現在對空輸入擋下，甲板以CLI export當前完整清單為來源並標明scope；骨架/殼保留凍結設計。
舵輪腳下有支持不代表上部不穿殼，舵neutral不撞也不代表轉動掃掠通。完整後驗與color guard清單見作品design/audits/20261014；不同資料源的封閉水下空氣（單船殼1397278／組裝1370871）不能混比。
大型組裝件的觀測/匯出串行；本輪並發讀曾讓一次export等不到鎖，確定零寫入後串行重匯。舊scratchpad可能不隨工作區帶來，關鍵獨立證據已收進作品git層。
初建build_structure.main/build_rig.main仍不是安全成船重跑入口。七梯問題是遊戲同高艏艉樓合層政策，隔離patch只是一份接入候選，不能因它通過便宣稱正式遊戲已採用。
