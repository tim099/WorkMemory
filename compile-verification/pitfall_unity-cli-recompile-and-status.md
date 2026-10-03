---
id: pitfall_unity-cli-recompile-and-status
topic: compile-verification
title: Unity CLI：頂層 recompile 失焦不編、status 量不到主執行緒卡住
type: pitfall
status: active
created_at: 2026-10-03
created_by: basecamp
links: []
related_docs: []
---

Unity 官方 CLI（1.0.0-beta.12＋com.unity.pipeline 0.8.0-exp.1）2026-10-03 在 Bar 實測的兩個坑：
1. 頂層 `unity recompile` 在 Editor 失焦、檔案有變動時回 `up_to_date` 而**沒編**（同一份變動 `senate cmd unity-recompile` 隨後抓到 stale_sources=1 並編譯）。要用 Editor 端的 `unity command recompile` → 輪詢 `recompile_status`：失焦也會編，有錯回 `failed=true`＋檔案行號錯誤碼。
2. `unity status` 不經過 Editor 主執行緒：主執行緒 Thread.Sleep(10000) 時每一筆照回 `ready`（心跳停 10.5 s）。⇒ 它不能當 Editor 存活訊號；`unity command editor_status` 會卡住（10.39 s），可以用「逾時沒回」當第二條讀數。
每次 CLI 呼叫約 1.5 s 啟動成本 ⇒ 不放進每次刷新都跑的路徑。讀數全文：ucl_core:Docs~/zh-Hant/Plan/Plan_Unity_CLI_Evaluation.md §5；候選清單 TASK-0390。
