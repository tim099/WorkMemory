---
id: decision_tavern-post-target-data-root
topic: senate-backend
title: Editor→Senate 發文用 target_data_root 選專案（不能叫 data_root）
type: decision
status: active
created_at: 2026-10-02
created_by: kotoko
links: []
related_docs: [Docs/Workflows/Tavern.md, src/Senate.Core/UnityTarget.cs]
---

Unity Editor 叫 Senate 的 `tavern-post`／`tavern-post-system` 時，用 **`target_data_root`** 選專案（`MorningLocalCmd.AcceptsTargetDataRoot` opt-in → `UnityTargetResolver.ResolveByDataRoot`），比不到啟用中的專案就擋、不退回預設。

為什麼不叫 `data_root`：CLI 的 `FillRootArg` 會替**宣告了** `data_root` 的 Cmd 自動補設定檔那一格，而補進去的值在 `SCP_CmdArgs` 裡被算成「顯式給的」（它改的是 raw args）⇒ 拿它選專案時，「--project Bar 卻沒帶 data_root」會被補成 LY 的根、跟專案名對打。一個沒有人替它補的新名字才分得出「呼叫端真的給了」。

後果要知道：Bar 目前沒在 senate.local.json 啟用 ⇒ Bar 的 Editor 發文會被擋（exit 2，確定沒發）——這是刻意的，比安靜落進 LY 的酒館好。Unity 端唯一入口 `UCL_TavernSenatePost`（共用 `UCL_PersonaProfileSenateBridge.RunCmd`，第四參數是資料根參數名）。
