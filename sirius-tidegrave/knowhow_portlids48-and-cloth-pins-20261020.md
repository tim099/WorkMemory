---
id: knowhow_portlids48-and-cloth-pins-20261020
topic: sirius-tidegrave
title: 48片砲門的新入口、移錨與帆旗固定點保留
type: knowhow
status: active
created_at: 2026-10-20
created_by: Sirius
links: []
related_docs: []
---

本輪完成潮墓號TASK-0743/0744/0745/0747。主船design/portlids_complete_20261019/README.md是48片砲門的新入口；舊design/tools/portlids.py仍是30片的篩選版，不要獨立draft/apply覆回。complete.py保留原30片及ID，新增18片；兩錨at.x560.5→563.5，model:ring引用讓錨纜跟隨。lower02 house6，lower05既有house7要保留。

外表面相對中位數偏差>3格只是篩選，平滑船殼斜度也可能被誤判；真正判定應做變換後開關與掃掠取樣。遊戲網格表面檢查會比9點voxel取樣多抓擦碰（lower S02只1點），按報告增house而非放寬檢查。完成48片開關、502繩端固定；船體1,033,347格checksum55c4e759、10帆不變。仍保留純視覺收砲對船穿入取樣1332點，不挖船殼解決它。

TASK-0744後桅lateen recipe.bind由[0,2]改[0]，保持3角pin；其證據design/lateen_20261019。TASK-0745船尾ensign只固定桿側角0/3，尾角1/2 pin=false，bind[3]保留；證據design/ensign_20261019。新砲門版本逐位元組保留這10帆，避免重匯回舊固定點。

Blockside reimport --no-build後VXK沒有端綁定；先build --raw供真實候選頁量，再act_ropebind(build=False)，最後壓縮build，可避免重複長建置。48片版shipcheck0錯誤，僅基線送藥手24/25警告；來源指紋49017ab492d58c0d，已實際測試全開全關。詳細模型/繩/帆變更範圍見asset_diff.json。
