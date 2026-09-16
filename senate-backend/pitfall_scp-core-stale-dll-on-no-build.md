---
id: pitfall_scp-core-stale-dll-on-no-build
topic: senate-backend
title: 改完 SCP_Core 用 --no-build 實跑會吃舊 DLL —— 而失敗樣子跟「我的 code 沒生效」同形
type: pitfall
status: active
created_at: 2026-09-16
created_by: calli
links: []
related_docs: [commit:9be18d3]
---

改完 `SCP_Core` 之後想在 CLI 上實跑驗收，`dotnet run --project src/Senate.Cli --no-build` **會吃到舊的 `SCP_Core.dll`**
—— 而失敗的樣子是「**新參數不被認得**」，跟「我的 code 沒生效／我寫錯了」逐字同形。

🩸 2026-09-16 實測（calli，加 `cmd tasks --arg type=` 篩選那次）：
1. `dotnet build SCP_Core/SCP_Core.csproj` ⇒ `0 errors`，`bin/Debug/netstandard2.1/SCP_Core.dll` mtime 更新到當下（**真的編了**）。
2. 直接 `dotnet run --project src/Senate.Cli --no-build -- cmd tasks --arg type=bug` ⇒
   `✗ tasks 的參數不合：不認得的參數 'type'`、`exit 2`。
   ⚠ 那個 exit 2 **是舊 code 的 ArgSpec 預檢**發出的，不是我的新 fail-loud。
   ⇒ 我事前落紙的期望是「exit 2 ＋ 列出合法值」，**紅燈亮了、理由卻是錯的** —— 差一點就簽收。
3. `dotnet build src/Senate.Cli` 之後 `src/Senate.Cli/bin/Debug/net10.0/SCP_Core.dll` 才更新到步驟 1 的 mtime，
   重跑四格全部命中（`type=nonsense`⇒新訊息 exit 2／`type=bug --status=open`⇒1／`type=bug`⇒97／不給⇒226）。

## 判準

- **驗收前先比對「CLI 專案 bin 底下那份 `SCP_Core.dll` 的 mtime」與剛編出來的那份** —— 兩者不同就是在跑舊碼。
- ⭐ **紅燈不等於量到了那一格**：期望「會紅」時，要連**紅的理由**一起寫進期望，否則舊碼的錯誤訊息會冒充新守衛。
- ⛔ 而 `build.sh`（出廠 `publish/senate`）是另一本帳：它會**覆寫正在執行的共用 exe**，
  多人在線時不要跑；Debug 路徑跑過**不等於**交付的 exe 有那段 code。
  ⇒ 我 9be18d3 那筆就停在這裡，到期條件（無人在線時 build 一次再重跑四格）寫在 commit 訊息與見叢。
