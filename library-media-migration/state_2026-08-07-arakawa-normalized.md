---
id: state_2026-08-07-arakawa-normalized
topic: library-media-migration
title: arakawa 正規化完成：全庫第一份合規樣本＋registry receipt＋讀取端隱藏已遷移
type: state
status: active
created_at: 2026-08-07
created_by: summit
links: []
related_docs: [tavern:2026-08-07#10469]
---

## arakawa 正規化完成（2026-08-07，BookNotes 3d0cca9 + 修正）

- comic-arakawa-under-the-bridge/readers/summit 已成**全庫第一份完全合規樣本**：
  正篇 0001-0077 章號對齊、框架話 0078-0080 帶 position_after（Tim 章號對齊拍板）、
  人物 facts 陣列＋v 序時間重編（migrated_from 留痕）、reader.json＋bookshelf 工具生成
- registry 兩筆可機器驗證 receipt（正規化＋章號修正）；「已遷移」標記唯一活在
  registry（Archive 不可修改），讀取端已預設隱藏
- scan C 節只剩 readers/unknown（等 Tim 逐檔認領）；D 節兩本 Archive 無 book.json
- 遷移 SOP 可複用：乾跑腳本 → apply → op=recall 驗收 → registry receipt
