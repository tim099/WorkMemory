---
id: pitfall_verify-the-right-copy
topic: senate-backend
title: 驗到的不是改的那份：sed 換掉 CRLF、PATH 上是出廠版 senate
type: pitfall
status: active
created_at: 2026-10-02
created_by: summit
links: []
related_docs: []
---

兩個「量到的不是我改的那一份」的坑（10-02）：
1. **Git Bash 的 sed -i 碰到 CRLF 檔會把整份行尾換掉**（UCL_LocalizedDocsManifest.txt：只想刪 2 行，diff --stat 顯示 368 行）。⇒ 改 CRLF 檔用 Edit 工具，或 python 以 bytes 讀寫；改完一律先看 `git diff --stat` 的行數再 stage。
   多行精確字串比對也常因縮排／符號編碼（例：⛔ 的位元組）對不上 —— 讓腳本在 assert 失敗時**不寫檔**，至少不會半套落地。
2. **PATH 上的 senate 是出廠版**：改了 Senate／SCP_Core 的 help 文字或行為，用 `senate cmd …` 驗會讀到舊的。⇒ 用 `src/Senate.Cli/bin/Debug/net10.0/senate.exe`（dotnet build 產物）驗，同時讓出廠版印一次當對照組。要讓 Server 上的常駐工作吃到新碼 ⇒ 必須 ./build.sh 出廠（會停掉共用 Server 再起回來；出廠前確認 Senate 樹上沒有別人未提交的改動），並用 server-ping 確認 build id 換了。
   需要直接呼叫 SCP_Core 方法（例如收 List／out 參數，invoke 接不了）⇒ 暫存區開一次性 console 專案引用 bin/Debug 的 SCP_Core.dll，跑完即丟。
