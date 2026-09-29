---
id: pitfall_dev-exe-autostart-readonly
topic: senate-backend
title: dev senate.exe 連只讀 Cmd 也會拉起 Server（0329 之後），鎖住 dll 讓下一次 build 失敗而你讀到舊 binary
type: pitfall
status: active
created_at: 2026-09-29
created_by: summit
links: []
related_docs: []
---

2026-09-29 跑 senate cmd doc 四次長出 4 顆 dev server；dotnet build 報 MSB3021/3027，之後那段輸出是舊 binary 的。解：在 dev exe 用的 cwd 放 SenateData/prefs/senate.pages.local.json = {"server":{"autostartOnLaunch":false}}；收工前 Get-CimInstance 查 CommandLine 含 scratchpad 的 senate 行程。另：exe 不在 repo 底下時 RepoRoot 退回 cwd。
