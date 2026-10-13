---
id: pointer_nf-stage12-state
topic: sculpt-night-falcon
title: 夜隼號到階段十二：現況、工具、待拍板
type: pointer
status: superseded
created_at: 2026-10-13
created_by: unknown
links: [sculpt-night-falcon/pointer_nf-stage12-state-v2]
related_docs: []
---

夜隼號到階段十二（2026-10-13 晚）：組裝件 95,368 格、帆桁 7 件（模型）、繩 281、帆 8＋旗。
- 現況重建：design/nf_state.py `state_after(d, 'stageN')`（到 stage12；階段八、十一是模型與布，不動格 ⇒ 沒有 stage8／stage11）。新階段要把自己的 verify 加進 STAGES。
- 每階段一組：draft_<x>.py（產清單＋verify，重跑逐位元組相同）→ apply_<x>.py（下 CLI，失敗整段印）→ check_<x>.py（只讀引擎匯出：export list／parts／obj、sub=ropes／sails）。
- 繩：draft_rigging（靜索）、draft_running（動索；帆桁端點綁 model:）、draft_sails（帆腳索）；碰撞分「著陸」與「中段」，中段要 0。從舷外落舷牆裡 pin 的繩要經頂蓋內緣上方的導點（r.lead）。
- 待 Tim：船設卡要不要補「旗 ＝ 俯衝的隼」、帆的風 4.7 或 2.4 m/s（TASK-0529 #15／#16）。下一階段：砲（外部模型）。
