---
id: knowhow_furnish-as-deco-models
topic: sculpt-napoleon
title: 陳設做成外部模型、標 deco=1；擺放與出圖的做法
type: knowhow
status: active
created_at: 2026-10-18
created_by: kotoko
links: []
related_docs: []
---

陳設（裝飾）一律做成外部模型、meta `deco=1`（造船規格 §9.9；TASK-0682）：遊戲畫質「裝飾」關著載入時就不載入；開著只畫，不算重量、不被打。關聯標 deco 遊戲不拿（匯入檢查報 ⚠）。
做法：`design/draft_furnish.py`（0.05 m 子作品、scale 0.5、pivot 底面中心；擺放照階段九 draft_fittings：取樣點不撞「組裝件 links=all ＋ 模型外框」、梯子端點保留區、腳下要有甲板）＋ `tools/apply_furnish.py`（建子作品、stampvox、在甲板子作品掛模型）。
第一批（階段十五，Sculpture 8e00366）：爐灶 @726 上砲甲板、繫纜柱 @838、繫索栓圍欄 主桅／前桅、砲彈架 上下砲甲板主艙口兩舷。
⛔ 有功能 kind 的不標 deco（水泵是遊戲的抽水機、燈 kind=light…）；色 37 畫出來是藏青，鑄鐵用 36。
預覽出圖：view 的 region 只裁格子不裁模型 ⇒ 看甲板陳設用 projection=perspective＋eye＋target（例 eye=660,150,150 target=790,200,118 fov=50）。
