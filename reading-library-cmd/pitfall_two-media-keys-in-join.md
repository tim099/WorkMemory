---
id: pitfall_two-media-keys-in-join
topic: reading-library-cmd
title: join 那一步有兩個 media 鍵：work slug vs 閱讀庫 media id（碰 Library 一律用後者）
type: pitfall
status: active
created_at: 2026-09-10
created_by: Sirius
links: []
related_docs: []
---

## 這條線上有**兩個**長得很像的 media 鍵，而它們在 join 那一步真的不同

- `aMedia`（session 的 `media_id`）＝ **work slug**，例：`made-in-abyss`
- `aLibId`（準備檔那把鍵 `prepared_key`）＝ **閱讀庫 media id**，例：`anim-made-in-abyss`

⇒ 任何要碰 `BookNotes/Library/media/<id>/` 的動作**一律用 `aLibId`**。
🩸 2026-09-10 實證：我在 `step=join` 掛登記時選了 `aLibId`，當晚那一場兩者真的不同 ——
回傳檔印「早就是 `anim-made-in-abyss` 的 reader」而 `media` 欄印 `made-in-abyss`。
⇒ 選 `aMedia` 的話會去登記一個**不存在的 media**（`RegisterReader` 會擋下並印 ⚠，
但那一格就永遠登記不了）。

## ⚠ 還沒修：`step=join` 回傳檔的 next 兩行印的是 `aMedia`

那兩行叫人用 `--arg media_id=made-in-abyss` 去跑 catchup 與 `note_chapter`／`bookmark`，
而 Library 底下只有 `anim-made-in-abyss`（收工回傳檔印的也是 `anim-` 那個）。
⇒ 照 join 那兩行走會撞「你還不是這部的 reader」；⛔ 更壞的是有人拿它去 `media_init` ⇒ 長出第二個平行 media。
📌 修法是那幾行改用 `aLibId`（一行的事，同一個檔）。⛔ 2026-09-10 沒做 —— Tim 沒點頭，而它動的是別人明天會讀到的輸出。

## 進場登記本身（TASK-0137 的 (C)，commit `a84687c2`）

`UCL_ReadingLibraryIO.RegisterReader(mediaId, persona, out created, out log, out error)`：
media.json 不存在 ⇒ **什麼都不建**回 false；已是 reader ⇒ **零寫入**回 true（冪等）；
anticipation 預設 3（進場時本人還沒讀，**工具不替他表態**）。
建 reader 的初值抽成 `EnsureReaderJson`，`MediaInit` 與它**共用同一份** —— ⛔ 不留第二份 schema。
掛在 `Cmd_StreamWatch` 的 `step=catchup` 與 `step=join`，**登記失敗不擋流程**。
