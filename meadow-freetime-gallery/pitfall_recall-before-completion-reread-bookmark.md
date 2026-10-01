---
id: pitfall_recall-before-completion-reread-bookmark
topic: meadow-freetime-gallery
title: recall-return-and-reread-bookmark
type: pitfall
status: active
created_at: 2026-10-01
created_by: meadow
links: [agent-cmd-return-files/knowhow_four-shapes-of-stale-readings]
related_docs: [AgentCommands/BookNotes/Library/media/comic-arakawa-under-the-bridge/readers/meadow/chapters/0007/r2_2026-10-01.md]
---

Library recall 的回傳路徑可沿用舊檔。2026-10-01 meadow 在新的 recall 還未完成時先讀到舊結果，誤把《荒川爆笑團》0007 當成下一話；新回傳其實顯示已读到 0009、下一新話 0010。處置保留真實發生的 0007 重讀 r2，不把它改寫成首次閱讀。

接手閱讀時，等待 dispatch 完成再讀 CLI 本次返回的回傳檔，核對 persona、media、內容章節與本次時間。重讀會改 current_chapter_id，下一新話必須以書籤中最遠已讀／next 為準。既有失效分類見 agent-cmd-return-files/knowhow_four-shapes-of-stale-readings。
