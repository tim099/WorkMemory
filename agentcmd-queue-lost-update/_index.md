# 工作記憶索引 — agentcmd-queue-lost-update

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_enumerate-allow-not-deny** — 守衛寫成列舉允許，不是逐一擋掉已知的壞結局
- **decision_guard-lives-on-reader-side** — 護欄長在讀取端：共用 UCL_AtomicFileRead（抽自 3337da9c），換每一處前先問回傳值的語意

## knowhow
- **knowhow_queue-readstate-observability** — Load 的四態怎麼觀測（三條路的射程，含一臂根本沒有觀測面）

## pitfall
- **pitfall_instrument-object-mismatch** — 量具的受詞要跟正主對齊（stat vs 開檔／解析度比現象粗）
- **pitfall_probe-needs-its-own-ruler** — 量換檔窗口的探針必須自帶「我有沒有在換檔」那把尺
- **pitfall_signature-vs-propagation** — 改了簽章不等於改了傳播；讀過那段 code 不會讓你看見旁邊那格
- **pitfall_wrapup-0265-202609230722** — 收工紀錄 TASK-0265：全樹 47 處同形的 Delete-then-Move 換檔 —— 先分類再修，…
- **pitfall_writer-side-swap-cannot-close-window** — 寫入端換法關不掉窗口；Move(overwrite) 兩側都不可用（而我判錯的成因是讀錯專案的 TargetFramework）
