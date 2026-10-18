---
id: pitfall_material-needs-reimport
topic: sculpt-napoleon
title: 改 material／smooth 要整份 reimport，metasync 帶不過去
type: pitfall
status: active
created_at: 2026-10-18
created_by: kotoko
links: []
related_docs: []
---

材質與平滑（關聯 meta material／render）是**匯出時烤進 .vxk 檔頭**的（header.materials、header.render），不是 meta。
⇒ 改了 material 或 render=smooth 之後，遊戲那側要**整份 reimport**（`admin.py reimport --id <船>`）；`metasync` 只換 meta 與固定點，讀數一格不變。
量法：Blockside 乾淨副本（git archive HEAD ＋ 工作區 _cache 複本）先 metasync → shipcheck「平滑渲染」那一行沒變；再 reimport → build → shipcheck 才變（2026-10-17 拿破崙號：平滑 28,900 → 325,182 格）。
另：shipcheck 讀的是 build 出來的成品，`reimport --no-build` 之後要再 build 一次報告才會更新。
