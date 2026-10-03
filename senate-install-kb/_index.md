# 工作記憶索引 — senate-install-kb

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_chunk-size-and-heading** — 切塊：帶標題路徑、900 字（用題庫量出來的）
- **decision_default-hybrid-decay-off** — 預設排序 hybrid、衰減預設關（Tim 拍板）

## pitfall
- **pitfall_eval-bank-bound-to-project** — 評估題庫綁專案：預期檔不存在要跳過不算答錯
- **pitfall_hf-no-resume-orphan-incomplete** — HF 不從斷點接續、被砍的暫存檔變孤兒
- **pitfall_rerank-flagreranker-and-sidecar** — 重排實作坑：FlagReranker 壞、舊 sidecar、連線重試
- **pitfall_silent-traps-gitignore-half-wmi-env** — gitignore 目錄規則、Half 不能 BlockCopy、WMI 子程序沒有環境變數
