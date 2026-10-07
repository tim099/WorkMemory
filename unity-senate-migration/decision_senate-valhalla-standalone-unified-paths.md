---
id: decision_senate-valhalla-standalone-unified-paths
topic: unity-senate-migration
title: Senate＋Valhalla 單獨運作、路徑一律走統一入口（10-07）
type: decision
status: active
created_at: 2026-10-07
created_by: basecamp
links: []
related_docs: []
---

**決策（Tim 2026-10-07）**：Senate 是 Server、Valhalla 是資料 repo，兩者搭起來單獨運作、**不依賴 Unity**；Unity 專案只是「標記施工目標」的選填欄位（PathsPage 顯示「Unity 專案根（選填）」）。路徑**一律走統一入口**，不准各處自行推導（「避免之後路徑需要調整時出錯」）。

**落點**
- 根的唯一定義：`SCP_PathRegistry`（描述表）；Senate 的值來源：`SenatePathBinding.StoredOf`；宿主插座只在 `SenateHostPaths.Install` 一處裝（CLI 與 senate-server.exe 共用）。
- 資料根唯一入口 `SenatePathBinding.ResolveDataRoot`；設定檔全域 `paths` 區塊（agentCommandsRoot／glossaryRoot／comicRoot），舊檔住在 projects[] 上的三格在 Load 時搬過去。
- 資料根底下版面唯一入口 `SCP_DataPaths`（Letters 會問宿主、Bank／Bartender／Memos／Rooms／ChatTavern）。
- 跟著 Senate 走的：`HostRepoRoot`（Senate 專案根，宿主給）→ 詞典 `Glossary/`（submodule）、自由時間活動 `SenateData/config/freetime_activities/`、kb 定義 `SenateData/config/kb_targets.json`。
- 酒館 refs／工作記憶 related_docs：資料根相對；舊 `AgentCommands/` 前綴讀時去掉。

**坑**
- 設定改 `auto` 時，**舊 exe** 會把字面 "auto" 當資料夾名（在 cwd 長出 `auto/`）—— publish 前設定要填完整路徑。
- 改設定檔格式（paths 區塊）要在 publish 之後：舊 exe 讀不到 paths 會退回推導、解到舊樹。
- 文件庫掃整個 `Docs/` —— 不要把非文件的 md 集合放進去（詞典放 Docs/Glossary 時撞名 sirius，skill 整支組不出來）。
- 「資料根的上一層＝Unity 專案」的推導已全部拿掉；剩 coding 退場閘（TASK-0463）。

**剩下**：TASK-0390 的 Unity CLI 接手 recompile、Bar 改名反向對照；TASK-0462（AutoCommitPage 根目錄）、TASK-0463（coding 閘照路徑驗）。
