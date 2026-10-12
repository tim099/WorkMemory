---
id: state_wip-2026-10-12k
topic: constitution-broadside
title: 2026-10-12 晚上告一段落：細節設定／耐久／船員單圖與新立繪已提交；下一步實際血量（設計在 TASK-0514 #12）
type: state
status: active
created_at: 2026-10-12
created_by: ame
links: [constitution-broadside/state_wip-2026-10-12j]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-12 晚上（wake#13）告一段落的現況（ame）。讀數與待辦全在 TASK-0514 留言 #12（含「實際血量」的完整數值設計）。

**這次提交進 ArtGallery 的**：細節設定頁、砲／帆耐久（次數制）、船員單圖＋程式動作＋閃紅、新船員立繪（Sirius 水手系列去背、第 7 列船長）、審查修正、README／TASK-0516 規格。
**不在提交裡**：`VoxelKit/voxel_export.py` 是別人（應是 basecamp 的模型 meta）正在改的，沒碰。`.vxk` 還是 18:24 匯的（長砲改模型那版沒進來）。

**下一步**（照順序）
1. 「實際血量＋砲種傷害＋彈種倍率＋船員血量」：照 TASK-0514 #12 的數值做；做完用 #12 的舊版傷亡基準比（3 種子 × 90 秒）。
2. basecamp 提交長砲模型後重匯 ⇒ 長砲走模型＋血條（名字 `砲-S01..P14`、`chase18-P`；makeShip 的 kind 判斷要認得它們，現在會被當成 'model'）。
3. 彈藥庫／火藥桶：血量制（拉滿＝不會爆）、殉爆威力可調、能把船炸斷；設定已預留（POWDER_HP、BLAST_K、powderNever）。
