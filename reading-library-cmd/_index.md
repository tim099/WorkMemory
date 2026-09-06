# 工作記憶索引 — reading-library-cmd

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_aclass-splits-into-two-families** — A 類分裂成兩族：Cmd_Books 同 store 可對拍／Cmd_Library 不同 store 也不同鍵 ⇒ 那四支不能用「同名 op」退場
- **decision_addbook-not-equal-media-init** — add-book ≠ Cmd_Library.media_init（不同 store）⇒ 仍是 B 類，且被 ②-bis 阻塞不在本輪移植  ↔ pitfall_booknotes-store-not-empty, state_inventory-20260906-library-py-35-subcmds
- **decision_python-recall-retire-gate** — Python reading-recall 退位閘：C# 補 facts + diff 只剩 generated_at 後直接刪（wrapper 否決）
- **decision_queue-failure-must-not-block** — 失敗的 Cmd 直接移除不堵塞 —— 但必須與 run_cmd 判定端成對改，否則失敗長得像成功
- **decision_rating-system-spec-2026-08-07** — 評分機制四輪定案：品質軸/口味軸分離、評分掛 round、單一 append-only overall_ratings[]（Tim 保留二次確認）  ↔ reading-trace-system, library-media-migration
- **decision_spec-2026-08-06-six-questions** — 六題定案：讀寫同框架在 C#、讀回自動、遷移要人、time_range 是事實
- **decision_tim-csharp-only-and-doc-sync** — Tim 鐵則：實作全在 C#（Python 只走 Cmd）+ 改 CMD 同步改 skill/文件不留舊版資訊

## pitfall
- **pitfall_booknotes-store-not-empty** — 「舊 BookNotes book.json store 已空」是錯的註解 —— 磁碟 157 份、活的 6 份、5 天內寫過 2 次  ↔ decision_python-recall-retire-gate
- **pitfall_recall-facts-false-empty** — C# recall facts 假滿值「（未登錄）」+ schema 隔夜快取 + persona 大小寫跨層不一致
- **pitfall_trigger-missed-during-domain-reload** — trigger 落在 domain reload 窗口被靜默漏接（RunCount=0 但 Editor 活著）
- **pitfall_wrapup-0143-202609061114** — 收工紀錄 TASK-0143：【主 Task】library.py 移植到 SCP_Core ＋ Senate…
- **pitfall_wrapup-0143-202609061234** — 收工紀錄 TASK-0143：【主 Task】library.py 移植到 SCP_Core ＋ Senate…

## state
- **state_aclass-behaviour-diff-tips-donations** — A 類行為對拍第一刀：tips／donations 資料層等價、字面層不等價（10/21 與 1/75 行不同，全是標點）
- **state_gura-progress-2026-08-07-goodnight** — gura wake#26 收工：評分機制拍板+文件化（零 code）；op=rate 未開工，10 項待定  ↔ reading-library-cmd/state_gura-cmd-library-q1-q6-feedback
- **state_inventory-20260906-library-py-35-subcmds** — library.py 35 支子命令退場盤點表（TASK-0143 ①）—— A 已有 C#/B 要移植/C 待議/D 零引用候選
- **state_progress-2026-08-07-goodnight** — 晚安快照：share/scan/瀏覽/隱藏/Cmd_Books/稿費全落地；明日 op=rate 施工  ↔ reading-library-cmd/state_progress-2026-08-07-share-live
- **state_tip-money-path-behaviour-diff** — 金流第一支 tip 對拍：欄位逐欄相同、行為相同；字面與檔名時戳不同（實測排序未壞，而沒壞是因為沒撞上）
- **state_gura-cmd-library-q1-q6-feedback** — gura 對 Cmd_Library 6 題討論的反饋與拍板 ~~[superseded]~~  ↔ reading-library-cmd/state_gura-progress-2026-08-07-goodnight
- **state_progress-2026-08-06-movie-pause** — 進度快照：三 op 已實測 / CJK 修正未驗 / 發文整合與管理頁未做 ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-07-day-end
- **state_progress-2026-08-07-day-end** — 進度快照：人物 op 與 brief 產檔完成，剩發文整合與管理頁 ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-06-movie-pause, reading-library-cmd/state_progress-2026-08-07-sirius-converged
- **state_progress-2026-08-07-share-live** — 進度快照：⓪①②收（11c1e9c）+稿費上線；剩③等Sirius過閘、④scan ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-07-sirius-converged, reading-library-cmd/state_progress-2026-08-07-goodnight
- **state_progress-2026-08-07-sirius-converged** — 進度快照：queue 根治收掉、1~4 與 Sirius 收斂、新前置=修 facts 假滿值 ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-07-day-end, reading-library-cmd/state_progress-2026-08-07-share-live
