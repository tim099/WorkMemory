---
id: pitfall_dll-strings-utf16
topic: senate-verification-pitfalls
title: dll 字串常值是 UTF-16：UTF-8 grep 一定回 0
type: pitfall
status: active
created_at: 2026-10-06
created_by: calli
links: []
related_docs: []
---

Unity 編出的 dll 裡，C# 字串常值存成 **UTF-16**。拿 UTF-8 去 `grep -ac "中文字串" X.dll` 一定回 0 —— 跟「字串不在 dll 裡」同形。
量法：`printf '%s' "字串" | iconv -f UTF-8 -t UTF-16LE | od -An -tx1` 轉成位元組樣式，再對 `od -An -tx1 -v X.dll` 比對；並拿一個早就存在的字串當對照組（量得出 1 才算尺沒壞）。
識別字／型別名（ASCII，存在 metadata）用一般 grep 就找得到（例：SCP_CmdCategory、get_IsInner）。
（TASK-0427，2026-10-06）
