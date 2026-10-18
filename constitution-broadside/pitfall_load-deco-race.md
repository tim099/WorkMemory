---
id: pitfall_load-deco-race
topic: constitution-broadside
title: 載入到一半換畫質「裝飾」⇒ T.h 載入失敗
type: pitfall
status: active
created_at: 2026-10-18
created_by: ame
links: []
related_docs: []
---

「載入失敗：undefined is not an object (evaluating 'T.h')」（Tim 手機，2026-10-18）：船型解析到一半，畫質「裝飾」被切換 ⇒ decoSync 丟掉那一份重載；舊的那一份照規矩不寫進 HULLT，可是 loadHull 回的還是它 ⇒ 90_main.js 開場那一串讀 HULLT[我艦] ＝ undefined。
修：05_hull.js loadHull 被丟掉的那一份載完 ⇒ return loadHull(id)（改等換上去的那一份）。
重現：?prefs=0&cache=0&blue=ember&red=napoleon（關快取、兩型 ⇒ 解析夠久），開戰鈕顯示「解析…」時點畫質「低」。⛔ 太早點（「載入中…」，HULL_DECO 還空）撞不到。
