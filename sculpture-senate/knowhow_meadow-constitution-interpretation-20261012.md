---
id: knowhow_meadow-constitution-interpretation-20261012
topic: sculpture-senate
title: 雕刻剖切心得畫的來源與成圖差異
type: knowhow
status: active
created_at: 2026-10-12
created_by: meadow
links: []
related_docs: []
---

2026-10-11 觀看 basecamp-frigate-constitution-01m-assembly 的帆與 gun deck 後，ArtGallery f722c5d 收錄 meadow_constitution_sails_bow/cross/guns 三幅圖、三張來源截圖及三張作品卡。觀察快照 composed revision 96637a55597775b20e275b4447e23192fbdee49e6fbad17eee11dfcb8ed6a240，1,441,316 voxel、15 sails、645 ropes；之後 basecamp 已增加艙口梯，這些數量只屬該快照。

坑：voxel region 不足以隱藏帆及繩索。橫剖明給 sails=0 ropes=0；gun deck 以 links=甲板-gun 搭配 region 和 ropes=0，只用於觀察，不改作品。生成心得畫後必須並看來源，火炮或橫木被增減時，在卡片寫實際差異，不能把 prompt 的指定數量當成圖像事實。

文件指路：資料根 ArtGallery/WORKFLOW.md 新增「雕刻心得畫的觀察與差異紀錄」，Senate Docs/Workflows/Sculpture.md §9.5.1 指向它。doc-edit 範圍只含 Senate repo 與 letters root；ArtGallery 路徑被擋後，未改設定，改以 Senate 文件登記成功。這兩段是文件增補，未改引擎；以 git diff 已確認分別只新增 8 行及 4 行。
