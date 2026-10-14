---
id: pitfall_zero-collision-search-start-must-collide
topic: sculpt-nightlamp
title: 退到零碰撞的起點要一定碰（v1 從空中起算、第一步就停）
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

「退到零碰撞」的搜尋，**起點必須是一定碰的位置**（砲架跨在舷牆上）。夜燈號 v1 從船外起算：整門砲在空中本來就零碰撞，第一步就停，22 門全在 Y 232、砲口在船外 44–56 格 —— 「全部零碰撞」「全部砲口在外」兩個讀數都是真的。
做法：起點 ＝ 砲尾在船殼外緣往裡 10 格，程式先斷言起點有碰到才開始退（place_guns.py）。同一類：任何「往某方向推到不碰為止」的搜尋都要先證明起點在碰。
另外：舷弧台階會讓砲在甲板面那一格退八十格都碰（第 1、11 門）⇒ 試 +1 格。
