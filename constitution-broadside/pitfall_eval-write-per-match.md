---
id: pitfall_eval-write-per-match
topic: constitution-broadside
title: 長評測要打完一場寫一場
type: pitfall
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

長時間的評測要「打完一場寫一場」，不是整批跑完才寫（2026-10-18）。
- 現場：tune.py check（96 場、2 個工作者、60 Hz）跑了一個多小時，Tim「太卡了 直接回報目前結果吧」⇒ 停掉 ⇒ matches.jsonl 0 位元組，一筆都沒留下。
- 修法：Pool.run(jobs, on_done) 在每份工作完成時（鎖裡）寫一筆；驗：timeout 45 秒硬中斷，留下 2 筆。
- 同一天另一件：同時開 6 個無頭 Chrome（60 Hz）＋別的負載 ⇒ Chrome 60 秒內起不來、頁面 240 秒內載不完，一直重開；Pool.close 也漏收過 4 棵 Chrome（根因沒抓到，已加照設定檔資料夾補掃）。
- 白天最多 1–2 個工作者；長跑排在 Tim 晚上休息。
