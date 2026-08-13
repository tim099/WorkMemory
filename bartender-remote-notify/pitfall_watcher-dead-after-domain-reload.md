---
id: pitfall_watcher-dead-after-domain-reload
topic: bartender-remote-notify
title: 指令通道靜默死亡：Watcher 的 Register 掛在 ModuleService 無 timeout 的 await 上，reload 後沒重新訂閱
type: pitfall
status: active
created_at: 2026-08-13
created_by: basecamp
links: []
related_docs: []
---

## 症狀
Editor 開著、看起來完全健康，但**所有 AgentCommand 一律不再被執行** ——
`pending.trigger` 躺在 queue 裡沒人撿（實測 ≥8 分鐘，正常 ~1s）。
Editor.log 沒有 error、沒有 warning、Unity 進程活著、記憶體穩定。

## 現場（2026-08-13 21:07-21:19，basecamp wake#57 實測）
- `_heartbeat.txt` **持續更新到當下** ⇒ 主執行緒活著在跳，最後一次停跳只有 4.47s（就是那次編譯）
  ⚠ 這條很重要：一開始我要報「Editor 凍結」，是心跳檔擋下這個錯判
- `_heartbeat_stalls.jsonl` 最後一筆 `13:07:29→13:07:33 gap=4.47s`（domain reload）
- Editor.log 在 `21:07:34` 之後完全沉默
- `[UCL_AgentCmdWatcher] Trigger detected` 最後一筆 = `21:07:18`（reload 之前）
- **`[UCL_AgentCmdWatcher] Registered.` 全 log 只出現 2 次，reload 之後一次都沒有**
- 同窗口 log 有 `UCL_ModulePathConfig.LoadConfig() string.IsNullOrEmpty(aJson)` ×4
  （呼叫鏈：DiscordMirrorDaemon.Tick → ReadRoutingTargets → UCL_Asset.GetAllIDs
   → ModuleService.Ins → Init → InitAsync → LoadConfig）

## 機制（讀 code 得到的耦合，非猜測）
`UCL_AgentCommandWatcher.Register()`（`UCL_AgentCommandWatcher.cs:98-105`）第一個動作是
`await UCL_ModuleService.WaitUntilInitialized(default)`，**之後**才 `EditorApplication.update += OnEditorUpdate`。

而 `WaitUntilInitialized`（`UCL_ModuleService.cs:802-810`）是
`UniTask.WaitUntil(() => m_Initialized)` —— **沒有 timeout、沒有 cancellation、失敗不出聲**；
`m_Initialized = true` 只在 `InitAsync` 的**最後一行**（`:941`）才設。

⇒ InitAsync 若在 941 之前 throw 或卡住，`m_Initialized` 永遠是 false
⇒ Register 永遠不訂閱 update ⇒ **整條 AgentCommand 通道靜默死亡**，
而心跳、daemon、Editor UI 全部照常 —— 板子看起來是綠的。

⚠ 未證實的一環：我沒有直接證據證明 InitAsync 這次確實停在 941 之前
（沒有 dump `m_Initialized`）。已證實的是「reload 後沒有 Registered.」＋「同窗口 ModuleService 噴錯」
＋「Register 的 await 掛在那個 flag 上」。這三條的交集指向它，但**不要寫成已證實**。

## 影響面（為什麼這隻比通知池那隻更嚴重）
它不是「通知不到人」，是**任何 Cmd 都不會執行** —— 發言、早安、晚安、commit 公告全部啞掉。
而 agent 這一端只會看到「run_cmd 等到 timeout」，看不到原因；
人這一端看到 Editor 一切正常。TRPG 卡住那幾次「沒發現原因」可能就是這個。

## 復原（目前唯一已知）
需要一次新的 domain reload 重跑 `[InitializeOnLoad]`（＝人去 focus Unity 觸發 import/recompile）。
**agent 從外部無法自救** —— 這也是為什麼它每次都要等人回來。

## 修法（未實作，等 Tim 拍板）
1. `WaitUntilInitialized` 加 timeout + 逾時 `LogError`（現在是無聲無限等）
2. Watcher 的 `Register()` **不該把訂閱掛在 ModuleService 初始化成功之上** ——
   訂閱是便宜且無害的，真正需要 ModuleService 的是 `GetTriggerPath()`；
   可以先訂閱、在 tick 裡才要求路徑（失敗就 warn 一次）
3. 心跳檔應該加一欄「watcher registered」—— 現在心跳證明主執行緒活著，
   卻不證明指令通道活著，而那正是「板子說沒事的時候有事」
