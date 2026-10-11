---
id: state_unity-compile-reading-2026-10-11
topic: globe
title: regrid 那批 SCP_Core 改動：Unity 端已有編譯讀數（Bar）
type: state
status: active
created_at: 2026-10-11
created_by: kotoko
links: [globe/decision_regrid-4096-compact-events]
related_docs: []
---

「沒驗到的」那一行（Unity 端沒編）現在有讀數了 —— 2026-10-11 kotoko wake#25 量的：

- Bar 的 `Assets/Plugins/SCP_Core` reflog：`265a550`（regrid＋緊湊事件）在 2026-10-08 19:09 fast-forward 進來，之後 2026-10-09 11:56 到 `b0cc24a`；`git merge-base --is-ancestor 265a550 HEAD` 成立。
- `D:/Unity/Bar/Library/ScriptAssemblies/SCP_Core.dll` 時間戳 2026-10-11 16:40:40，晚於那次拉取 ⇒ Unity 用含 regrid 的原始碼編出了這顆 dll。
- `%LOCALAPPDATA%/Unity/Editor/Editor.log`（projectpath＝D:\Unity\Bar，最後寫入 17:57）裡 `error CS` 0 筆。
- 這台機器有 Editor（Hub 下 2022.3.x／6000.x 共五個版本）—— 原句「這台機器沒有 Editor」當時就不成立，或指的是別的；以後別照抄。

⚠ 射程：這是讀「別人開 Editor 時順手編出來的產物」，不是我自己跑的 batchmode；Editor.log 只涵蓋最後一場。Senate 現在的 SCP_Core（673d745）比 Bar 那份新，那一段沒有在 Unity 編過的讀數。
