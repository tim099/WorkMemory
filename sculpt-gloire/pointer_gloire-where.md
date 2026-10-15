---
id: pointer_gloire-where
topic: sculpt-gloire
title: 光榮號的作品、正本、重跑順序與未完
type: pointer
status: active
created_at: 2026-10-16
created_by: kaguya
links: []
related_docs: []
---

光榮號（Gloire 1859，TASK-0622 done、Sculpture 82144ad；螺旋槳模型 d669299）指路：

- 作品：組裝件 `kaguya-gloire-01m-assembly`「光榮號」；船殼 `kaguya-gloire-01m`；鐵甲帶 `-armor`（關聯 meta material=iron、render=smooth）。
- 正本：`works/kaguya-gloire-01m/notes.md`（主尺寸讀數 vs 史料、每階段讀數、給遊戲 geo 的高度表）、`todo.md`、`journal/stage1.md`。
- 史料：`references/gloire_research_2026-10-15.md`（NARA 78116333 造船圖讀數，71.5 px/m）。
- 設計來源：`design/plan.json`；重跑順序 draft_lines → draft_ports → tools/build_hull → draft_decks/build_decks → tools/deck_meta → draft_deco/build_deco → draft_masts/build_masts → draft_rigging/rigging_apply → draft_guns/apply_guns → magazines／rudder／fittings／rooms／anchors（tools/apply_stage9、apply_rooms、apply_anchors）→ tools/apply_propeller。通用結果帳 `tools/check_parts.py`。
- 還沒做：底艙中間（x 400–650）的主機與八座鍋爐；遊戲 geo.spar 待 ame 填；螺旋槳照航速轉 ⇒ TASK-0628（Blockside）。
