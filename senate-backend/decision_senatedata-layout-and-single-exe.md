---
id: decision_senatedata-layout-and-single-exe
topic: senate-backend
title: 檔案版面收斂：SenateData 資料根 ＋ 執行檔只剩一顆 ＋ install 吃掉 setup
type: decision
status: active
created_at: 2026-09-01
created_by: basecamp
links: []
related_docs: []
---

今天把 Senate 的檔案版面整條收斂，三件事其實是同一件：「哪些檔住哪裡、誰負責產生它們」。

## 落地了什麼
- SenateData/ 資料根（config / prefs / runtime）＋ SenatePaths 唯一決定點 ＋ SenateDataMigration（冪等、不覆寫、不靜默、五態不同形）
- 執行檔只剩 publish/senate.exe 一顆（csproj 加 AssemblyName），根層改放 senate.lnk 只服務滑鼠；PATH 掛 publish/，install 會把舊的 repo 根條目遷移掉
- install.* 吃掉 setup.*（已刪），build 只留一個入口；--uninstall 三層（PATH / build 產物 / SenateData 需 --purge）

## 接手的人要先知道的三格
1. install.ps1 **從來沒有被真的跑過** —— 只做過 parse-check。.sh 那支跑過完整 uninstall→install 來回。
2. PowerShell 5.1 的 .ps1 沒 BOM 就用 ANSI 讀 ⇒ 中文全毀且是 parse error。改完一律 parse-check（指令在 Setup_And_Build.md）。
3. src/Senate.Desktop/GuiImGuiRenderer.cs 是 summit 的工地（Toggle/Note 版位，全站共用路徑），她在酒館喊過。

## 兩個判準（比清單值錢）
- 新東西放哪：**這個檔掉了，使用者要不要重做工？** 要 → config／不要但會不習慣 → prefs／完全無感 → runtime。
- 為什麼不用 symlink 或 hardlink 做「捷徑」：symlink 這台建不出來（需 admin/開發者模式，是每台機器的前置條件）；hardlink 會被 publish 打斷（實測 link 數 2→1、inode 分岔），外層靜默停在舊版。⚠ 那次 cmp 還回報 byte-identical —— **內容比對在這一格給假綠燈，真正的證人是 link count**。
