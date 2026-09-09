---
id: pitfall_assets-scp-core-binobj-cs1704
topic: senate-backend
title: Assets 底下的 SCP_Core 長出 bin/obj ⇒ CS1704 同名組件，把別人的 Unity 編譯弄紅（偵測一行；成因未解）
type: pitfall
status: active
created_at: 2026-09-09
created_by: basecamp
links: []
related_docs: [Assets/Plugins/SCP_Core/Docs~/Coding_Standards.md, tavern:2026-09-09#17015, task:TASK-0123]
---

## 現場（2026-09-09，basecamp）

`Assets/Plugins/SCP_Core/` 底下長出 `bin/` `obj/`，Unity 替它們生了 `.meta`（09:35:25）⇒
**Unity 同時吃到原始碼與那顆 DLL**：

```
error CS1704: An assembly with the same simple name 'SCP_Core' has already been imported
             （…'D:\Unity\LY\Assets\Plugins\SCP_Core\obj\Debug\netstandard2.1\…'）
09:35:21  Errors 1 / Warnings 3178      ← LY 的 Unity 編譯壞了
09:42:xx  Errors 0 / Warnings 121       ← 刪掉那四個之後
```

⚠ **而當時 @calli 正在同一個 Editor 裡工作** —— 這個坑的代價不是我自己卡住，是**把別人的 Editor 弄紅**。

## 規則本體早就寫著（不是新發現）

`<SCP_Core>/Docs~/Coding_Standards.md` §4.7 末節：**掛在 `Assets/` 底下的那一份不要 `dotnet build`。**

## ⭐ 這一格要補的是**偵測與歸因**，不是規則

- **偵測便宜到不用記**：`ls Assets/Plugins/SCP_Core/bin` —— 有東西就是它。
  修法＝刪 `bin/ obj/ bin.meta obj.meta`（前兩個 gitignore、後兩個 untracked ⇒ 刪掉是安全的），然後 `unity-recompile`。
- 🩸 **而「是哪一道指令生出來的」我沒查到，標未解**：
  · 我的 `dotnet build SCP_Core.csproj` 跑在 `Senate/SCP_Core`（另一份工作副本，它的 dll 是另一顆）
  · 兩份**不是** junction（`ls -di` inode 不同）
  · `Senate.slnx` 只引用 `SCP_Core/SCP_Core.csproj`（Senate 那份）
  · LY 那顆 dll mtime `09:32:02`，而我那個時窗沒有一支指令的 cwd 在那份底下
  ⇒ ⛔ **不編一個說得通的成因。** 下一個撞到的人請先量「誰在那裡跑過 dotnet」。

## 判準（可照做的那個）

改完 SCP_Core、**在跑任何 `dotnet` 之前先問「我的 cwd 在哪一份工作副本」**；
而收工前跑一次那行 `ls` —— 它比記住規則便宜，也比 CS1704 便宜。
📌 因為這隻的失效樣子不是「我編不過」，是**別人的 Editor 紅了而他不知道為什麼**。
