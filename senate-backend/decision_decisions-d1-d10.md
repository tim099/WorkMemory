---
id: decision_decisions-d1-d10
topic: senate-backend
title: 十條拍板（含兩條被實測改掉的）
type: decision
status: active
created_at: 2026-08-23
created_by: basecamp
links: []
related_docs: [D:/Unity/Senate/Docs/Logs/Decisions.md]
---

拍板紀錄住在 Docs/Logs/Decisions.md（規矩：新決策往下加、不改舊條目、被推翻的留著並標註被誰取代）。要點：D2 Senate 是真相源但『那句話本身不會讓任何事發生』——真正讓 Unity 不再是真相源的是『Unity 端改成讀 JSON』那行碼；D3 共用碼寫 Unity 方言（C#9/netstandard2.1/零套件），護欄是 csproj 的 LangVersion 9.0 與 asmdef 的 noEngineReferences，兩道方向相反；D4 JSON 照概念重寫不逐字搬（逐字搬會拖進 UCLI_CopyPaste 與 JsonData 裡的 OnGUI）；D5 UI 走中間層，價值不是換畫布而是『UI 有讀數』；D6 顯式 key 逐字採用（第一版把它丟進路徑推導，搬進 Row 就換 id）；D7→D9 我把『包原生 DLL 的單檔開不了窗』推廣成『不要用 single-file』——錯，換一個旗標就通（判準：這個做法不行與這個旗標不行是兩件事）；D10 .ps1 一律 UTF-8 with BOM（PS5.1 沒 BOM 用 ANSI 讀，整支 parse error，而我全程用 Git Bash 測所以看不到）。
