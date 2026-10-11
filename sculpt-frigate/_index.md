# 工作記憶索引 — sculpt-frigate

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_height-frame-no-drag** — 高度的框：11 號 body plan 沒畫 drag，sheer plan 讀數要換框
- **decision_keel-calibration** — 02 號側面圖校準組與龍骨讀法（階段一）
- **decision_target-detail-section-photo** — 目標細緻度：Tim 貼的剖面模型照（甲板、橫樑、柱、桅杆插到內龍骨、壓艙石）

## knowhow
- **knowhow_reference-drawings** — 參考圖紙清單（NARA RG 19）與龍骨斷面讀數
- **knowhow_scipy-interpolate-fixed** — scipy.interpolate 已修好（刪掉 2023 殘留的 interpnd.pyd）
- **knowhow_wrapup-0478-202610101453** — 收工紀錄 TASK-0478：精緻風帆戰艦（0.1 公尺／voxel，照圖紙從龍骨起建）
- **knowhow_wrapup-0478-202610101512** — 收工紀錄 TASK-0478：精緻風帆戰艦（0.1 公尺／voxel，照圖紙從龍骨起建）
- **knowhow_wrapup-0478-202610101639** — 收工紀錄 TASK-0478：精緻風帆戰艦（0.1 公尺／voxel，照圖紙從龍骨起建）

## pitfall
- **pitfall_stage6-carve-lessons** — 階段六雕之前修與雕刻的坑（跨站 nan、接縫台階、雙片一塊產、先刻再貼）  ↔ sculpt-frigate/pitfall_stage6-precarve-verifier
- **pitfall_stage6-precarve-verifier** — 階段六雕之前 verifier 抓到的坑（讀數照抄計畫、HullV2 nan、中線縫） ~~[superseded]~~  ↔ sculpt-frigate/pitfall_stage6-carve-lessons
