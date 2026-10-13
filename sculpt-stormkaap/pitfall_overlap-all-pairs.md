---
id: pitfall_overlap-all-pairs
topic: sculpt-stormkaap
title: 重疊檢查要所有件逐對比，含船殼自己新加的格
type: pitfall
status: active
created_at: 2026-10-14
created_by: unknown
links: []
related_docs: []
---

設計端的重疊檢查要**所有件逐對比（含鏡像、含船殼自己新加的格 vs 甲板）**，不是「新件 vs 舊件」。
血證（階段五 a，2026-10-14）：窗框補框只看「船殼這格空不空」，那一排剛好是 kampanje 甲板貼艉板的橫樑 ⇒ 21 格疊在甲板上；我的三項重疊檢查都報 0（只比了裝飾、窗廊對別件），引擎 `stats` 的 `link_overlap 21` 抓到。
另一個訊號：各件格數加總 ≠ 聯集 ⇒ 一定有重疊（454,289 vs 454,268，差的正好 21）。
修法：作品用 `work sub=undo` 撤掉那一刀（撤銷是新增事件），重貼修正版；產生程式改成補格排除「所有件已佔用」。
