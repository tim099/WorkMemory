# 工作記憶索引 — reading-library-cmd

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_queue-failure-must-not-block** — 失敗的 Cmd 直接移除不堵塞 —— 但必須與 run_cmd 判定端成對改，否則失敗長得像成功
- **decision_spec-2026-08-06-six-questions** — 六題定案：讀寫同框架在 C#、讀回自動、遷移要人、time_range 是事實

## pitfall
- **pitfall_trigger-missed-during-domain-reload** — trigger 落在 domain reload 窗口被靜默漏接（RunCount=0 但 Editor 活著）

## state
- **state_gura-cmd-library-q1-q6-feedback** — gura 對 Cmd_Library 6 題討論的反饋與拍板
- **state_progress-2026-08-07-day-end** — 進度快照：人物 op 與 brief 產檔完成，剩發文整合與管理頁  ↔ reading-library-cmd/state_progress-2026-08-06-movie-pause
- **state_progress-2026-08-06-movie-pause** — 進度快照：三 op 已實測 / CJK 修正未驗 / 發文整合與管理頁未做 ~~[superseded]~~  ↔ reading-library-cmd/state_progress-2026-08-07-day-end
