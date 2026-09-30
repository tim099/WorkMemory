---
id: pitfall_buildsh-failure-leaves-server-down
topic: unity-senate-migration
title: build.sh 失敗不會把停掉的 Server 拉回來；它也會把別人的半成品一起編進去
type: pitfall
status: active
created_at: 2026-09-30
created_by: basecamp
links: []
related_docs: []
---

**出貨 senate（`build.sh`）的兩個坑（2026-09-30 basecamp 實地）。**

1. **失敗路徑不會把 Server 拉回來。** build.sh 先落旗標、停掉 main／tavern 兩顆 Server，再 publish；publish 撞鎖失敗時，trap 只收旗標，**停掉的 Server 保持停著**。成功路徑才會重啟。
   - 那次的鎖是一顆**短命的同事 CLI**（`senate.exe` pid 36300，查的時候已經退出）剛好握著 `publish/senate.exe`。
   - 處置：馬上 `./publish/senate.exe server start --detach --id main|tavern`（舊 exe 還在，GenerateBundle 是刪檔那一步失敗）。那段期間已經有人的發文觸發 autostart 把 main 拉起來了。重跑 build 前先 `tasklist //FI "IMAGENAME eq senate.exe"`。
   - ⛔ 沒修 build.sh（出貨腳本，改不改由 Tim 決定）。
2. **build.sh 編的是整棵工作區**，含別人未提交／未追蹤的檔 ⇒ 出貨前先看兩棵的 `git status`（Senate 與 SCP_Core）＋施工場（`AgentCommands/sessions/*.json` 的 Coding kind 與 scope）。有人在施工就先協調，⛔ 不替別人出貨半成品。
- 驗 publish 真的生效：`server list` 的 build id；`selftest` 要對 **publish 的 exe** 跑，不是 Debug DLL。
