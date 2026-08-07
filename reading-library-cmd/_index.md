# 工作記憶索引 — reading-library-cmd

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_python-recall-retire-gate** — Python reading-recall 退位閘：C# 補 facts + diff 只剩 generated_at 後直接刪（wrapper 否決）
- **decision_queue-failure-must-not-block** — 失敗的 Cmd 直接移除不堵塞 —— 但必須與 run_cmd 判定端成對改，否則失敗長得像成功
- **decision_rating-system-spec-2026-08-07** — 評分機制四輪定案：品質軸/口味軸分離、評分掛 round、單一 append-only overall_ratings[]（Tim 保留二次確認）  ↔ reading-trace-system, library-media-migration
- **decision_spec-2026-08-06-six-questions** — 六題定案：讀寫同框架在 C#、讀回自動、遷移要人、time_range 是事實
- **decision_tim-csharp-only-and-doc-sync** — Tim 鐵則：實作全在 C#（Python 只走 Cmd）+ 改 CMD 同步改 skill/文件不留舊版資訊

## pitfall
- **pitfall_recall-facts-false-empty** — C# recall facts 假滿值「（未登錄）」+ schema 隔夜快取 + persona 大小寫跨層不一致
- **pitfall_trigger-missed-during-domain-reload** — trigger 落在 domain reload 窗口被靜默漏接（RunCount=0 但 Editor 活著）

## state
- **state_gura-progress-2026-08-07-goodnight** — gura wake#26 收工：評分機制拍板+文件化（零 code）；op=rate 未開工，10 項待定  ↔ reading-library-cmd/state_gura-cmd-library-q1-q6-feedback
- **state_progress-2026-08-07-goodnight** — 晚安快照：share/scan/瀏覽/隱藏/Cmd_Books/稿費全落地；明日 op=rate 施工  ↔ reading-library-cmd/state_progress-2026-08-07-share-live
- **state_gura-cmd-library-q1-q6-feedback** — gura 對 Cmd_Library 6 題討論的反饋與拍板 ~~[superseded]~~  ↔ reading-library-cmd/state_gura-progress-2026-08-07-goodnight
- **state_progress-2026-08-06-movie-pause** — 進度快照：三 op 已實測 / CJK 修正未驗 / 發文整合與管理頁未做 ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-07-day-end
- **state_progress-2026-08-07-day-end** — 進度快照：人物 op 與 brief 產檔完成，剩發文整合與管理頁 ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-06-movie-pause, reading-library-cmd/state_progress-2026-08-07-sirius-converged
- **state_progress-2026-08-07-share-live** — 進度快照：⓪①②收（11c1e9c）+稿費上線；剩③等Sirius過閘、④scan ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-07-sirius-converged, reading-library-cmd/state_progress-2026-08-07-goodnight
- **state_progress-2026-08-07-sirius-converged** — 進度快照：queue 根治收掉、1~4 與 Sirius 收斂、新前置=修 facts 假滿值 ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-07-day-end, reading-library-cmd/state_progress-2026-08-07-share-live
