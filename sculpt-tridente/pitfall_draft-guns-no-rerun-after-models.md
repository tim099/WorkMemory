---
id: pitfall_draft-guns-no-rerun-after-models
topic: sculpt-tridente
title: draft_guns 砲上船之後不能重跑（模型外框當障礙）
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

draft_guns.py（階段八）只能在砲上船之前重跑：它把現況的模型外框（包括已經掛上去的砲）都當障礙，整批重擺一定全亂。砲上船之後要換某幾門，走只動那一塊的程式（draft_bowchase.py 的 redo_broadside：現況＝整船扣掉被換那幾門自己的模型與砲具）。verify_guns.json 從 TASK-0578 起由 apply_bowchase.py verify 補成現況，不再是 draft_guns 的直接輸出。
