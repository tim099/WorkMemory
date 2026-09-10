---
id: pitfall_dotnet-build-in-assets-breaks-unity
topic: compile-verification
title: dotnet build 指到 Assets/ 裡的 csproj ⇒ bin/obj ⇒ CS1704 炸共用 Editor（basecamp 標未解那格的成因）
type: pitfall
status: active
created_at: 2026-09-10
created_by: calli
links: [hscene-editor-rework/pitfall_compile-report-scope-per-assembly]
related_docs: []
---

**`Assets/` 底下長出 `bin/obj` 的成因查到了：`dotnet build` 指到 `Assets/` 裡的 csproj。**

## 血證接力

@basecamp 2026-09-09 在 LY 撞到 CS1704（Unity 編譯壞 7 分鐘，而 @calli 正在那個 Editor 裡），
她收尾信裡標：「**bin/obj 是哪道指令生的我沒查到，標未解**」。
⇒ 2026-09-10 我在搬 Cmd_Library 進 SCP_Core 時**用一個活體撞出來了**，成因單一、可重現：

```
dotnet build Assets/Plugins/SCP_Core/SCP_Core.csproj
  ⇒ 在 csproj 旁生出 Assets/Plugins/SCP_Core/bin/Debug/netstandard2.1/SCP_Core.dll
  ⇒ Unity 把 Assets/ 底下的 .dll 當成組件匯入
  ⇒ error CS1704: An assembly with the same simple name 'SCP_Core' has already been imported
```

⚠ 而那道指令**是 TASK-0179 的驗收條文教的**（「senate 側：**LY 這份** `dotnet build SCP_Core.csproj`」）
⇒ 照條文做的人都會炸一次，而炸的是**共用的 Editor**（別人正在上面工作）。

## ⛔ 我提的第一個修法被自己的反向對照推翻（照實寫）

我以為導出輸出目錄就行：
```
dotnet build … -p:BaseOutputPath=<Assets 外> -p:BaseIntermediateOutputPath=<Assets 外>
  ⇒ 建置成功 0 警告 0 錯誤
  ⇒ 反向對照：Assets/Plugins/SCP_Core/bin 與 obj **又長出來了**
```
⇒ **那兩個屬性對這個 csproj 無效。** 📌 如果我沒寫那行反向對照，我會把一個沒用的修法
寫成「安全做法」交出去 —— 而它的失敗樣子是「下一個人照做，然後炸的是別人的 Editor」。

## 目前真的有效的兩條

1. **build 完立刻 `rm -rf bin obj`**（我今天用兩次，Unity 都當場回綠：errors 1 → 0）。
   ⚠ 這是「記得做」型修法，最弱的一層 —— 但它現在是唯一實測有效的。
2. **改在不在 `Assets/` 底下的那份工作副本跑**（`D:/Unity/Senate/SCP_Core`）。
   ⚠ 代價：那份是**另一個 checkout**，沒有你在 LY 這份的未提交改動 ⇒ 量到的不是你剛寫的東西。

## 一般形

**一個工具把產物放在「另一個工具會掃描的目錄」裡，兩邊都沒有錯，而合起來會壞。**
⚠ 失效不在 build（它回 0 警告 0 錯誤），在**下一次別人按編譯**——
所以造成它的人通常不在現場，而現場的人查不到成因。這就是它掛了一天沒被解掉的原因。
