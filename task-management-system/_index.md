# 工作記憶索引 — task-management-system

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_contract-task-memory** — 契約：Task ↔ 工作記憶（欄位／不互寫／不一致只印／錨點在 Task 檔）
- **decision_pm-decisions** — 拍板：PM 職責邊界與主 Task 怎麼撐
- **decision_rulings-20260824** — 拍板四題：memory_topic 單值／久未更新用 Task.updated_at／QA 閘不擴大但要出聲／施工順序
- **decision_tim-rulings-20260825** — Tim 2026-08-25 三條拍板：全系統 UTC／收工閘看本次醒來不看曆／探針不開單
- **decision_trigger-timing** — 拍板：工作記憶的四個觸發點（讀在回看 task 時、不掛早安）
- **decision_wrapup-0019-202608270936** — 收工紀錄 TASK-0019：op=wrapup 收工（進度→Task／為什麼→記憶）＋ 晚安收工閘（擋但跳過…
- **decision_wrapup-and-sleep-gate** — 拍板：op=wrapup 收工（一動作兩目的地）＋ 晚安收工閘（擋但跳過留名）

## knowhow
- **knowhow_csharp-side-boundaries** — C# 那半的四塊落地與各自沒驗到的那一格（summit）

## pitfall
- **pitfall_known-traps** — 五個坑（每個都不報錯）
- **pitfall_rmw-no-await-premise** — Cmd_Task 的併發安全靠「RMW 無 yield」這個沒有語法在保護的前提
- **pitfall_watch-out-20260825** — 動這套系統前要知道的七件事（注意事項，非進度）
- **pitfall_wrapup-0005-202608240716** — 收工紀錄 TASK-0005：文件與企劃：RFC/Workflow 對齊「早安零改動」拍板，並與 P0/P1 …
- **pitfall_wrapup-0008-202608240721** — 收工紀錄 TASK-0008：【主 Task】跨 agent 任務系統（UCL_Task）
- **pitfall_wrapup-0016-202608240716** — 收工紀錄 TASK-0016：記憶流程進文件與 Skill（三格分流＋跨多日接回章）
- **pitfall_wrapup-0019-202608270123** — 收工紀錄 TASK-0019：op=wrapup 收工（進度→Task／為什麼→記憶）＋ 晚安收工閘（擋但跳過…
- **pitfall_wrapup-0019-202608270936** — 收工紀錄 TASK-0019：op=wrapup 收工（進度→Task／為什麼→記憶）＋ 晚安收工閘（擋但跳過…
- **pitfall_wrapup-0021-202608240705** — 收工紀錄 TASK-0021：收工閘探針 B（驗 skip_reason 留名）
- **pitfall_wrapup-20260824** — 收工 2026-08-24：我自己造的兩隻、三次讀數過期、四個被否決的選項

## state
- **state_current-board** — 看板快照與關鍵路徑（接手先讀這格） ~~[superseded]~~

## pointer
- **pointer_where-things-are** — code 與文件在哪
