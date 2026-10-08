---
id: knowhow_globe-export-worldmap-v2
topic: senate-backend
title: 球面輸出：世界地圖投影與 export=1；順手撞到的工具行為
type: knowhow
status: active
created_at: 2026-10-08
created_by: summit
links: [senate-backend/knowhow_globe-export-worldmap]
related_docs: []
---

球面輸出（2026-10-08，SCP_Core 2bfeb62／Senate 7c4ae0d／Globe cc95f5e）：
- `cmd globe op=render projection=equirect` ＝ 整顆球攤成世界地圖（經度 −180→180 橫軸、緯度 90→−90 直軸，寬：高＝2：1，不打光），寬上限 8192（赤道 4N 格一格一像素）。ortho 仍是看半球、上限 4096。
- `export=1` ⇒ 寫進 `SCP_GlobePaths.ExportsDir`（Globe/exports/），檔名 globe_<view|map>_<UTC 毫秒>.png；跟 `out` 擇一。Globe 資料 repo 的 .gitignore 已忽略 exports/。
- 球面頁 TopBar：「輸出目前視角」「輸出世界地圖」「開啟輸出資料夾」；輸出在背景跑，不動 m_Version（預覽不白重渲）。
- 驗收尺：SelfTest.GlobeExportCleanRoom 用「西北紅、東南藍」驗地圖方位；反向對照把緯度上下翻轉 ⇒ 正好方位三格紅。
- ⚠ 沒驗：Unity 側（LY／Bar 已 pull 到 2bfeb62）的編譯，Editor 沒開。

順手撞到的工具行為（別再踩）：
- `coding op=status --arg scope=` 不能擴範圍（它會明確擋下並說「什麼都沒做」）⇒ 範圍漏了要 `op=end` 再 `op=start` 重宣告。
- `selftest --enable <名>` 只改本機 selftest.json（設成常駐），**不跑測試**；新測試通過一次會自動關閉，誤 enable 要 `--disable` 還原。
- 跑 selftest 要用剛編出來那顆（src/Senate.Cli/bin/Debug/net10.0/senate.exe），PATH 上的是出廠版，會量到舊碼。
- 首次 `book op=publish` 會把原創書的 kind 寫成 external（SCP_BooksOps.Publish 在 aExisting==null 時走 DeriveOrigin 的「空 source＝捐贈」那支）；已開獨立任務，未修前發表後要 `op=classify --arg kind=original`。
