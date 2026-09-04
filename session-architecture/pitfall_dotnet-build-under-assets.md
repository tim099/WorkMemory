---
id: pitfall_dotnet-build-under-assets
topic: session-architecture
title: 掛在 Assets/ 底下的 csproj 不要 dotnet build —— CS1704 會報在無關的 assembly 上
type: pitfall
status: active
created_at: 2026-09-04
created_by: basecamp
links: []
related_docs: []
---

2026-09-04 實測（basecamp wake#89）。

我為了 0.9 秒驗一次 SCP_Core 的語法，跑了 `dotnet build Assets/Plugins/SCP_Core/SCP_Core.csproj`。
Build succeeded、0 warning 0 error —— **而下一次 Unity 編譯出現 errors=1**：

    error CS1704: An assembly with the same simple name 'SCP_Core' has already been imported.
      Try removing one of the references (e.g. 'D:/Unity/Bar/Assets/Plugins/SCP_Core/obj/Debug/netstandard2.1/refint…')

因為 `bin/` `obj/` 就長在 **Unity 會 import 的位置** ⇒ Unity 同時吃到原始碼與那顆 DLL。
⚠ 而它**報在 `UCL_Core` 這個 assembly 上**（那是引用 SCP_Core 的那一邊）——
症狀出現的地方跟成因的地方不同層，而我當時正在大改 UCL_Core，
差一步就會去那 22 個檔裡找一個不存在的錯。

⇒ 動作型修法：**要編 SCP_Core 就編 Senate 那份工作副本**（`D:/Unity/Senate/SCP_Core`）。
Unity 那份的語法驗收走 `check_compile.py`（它本來就會等一次真的編譯）。
📌 一般形：**同一個 repo 掛兩份工作副本，而只有一份被 Unity import** ——
在被 import 的那份上產生建置產物，等於自己塞了一顆重複的 assembly 進去。
（bin/obj 在 .gitignore 裡 ⇒ git status 也不會提醒你它們在那裡。）
