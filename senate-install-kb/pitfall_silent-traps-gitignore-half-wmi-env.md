---
id: pitfall_silent-traps-gitignore-half-wmi-env
topic: senate-install-kb
title: gitignore 目錄規則、Half 不能 BlockCopy、WMI 子程序沒有環境變數
type: pitfall
status: active
created_at: 2026-10-02
created_by: kaguya
links: []
related_docs: []
---

三個「不出聲」的坑（2026-10-02 安裝系統＋知識庫）：
1. `.gitignore` 寫 `/SenateData/` 會擋整個目錄 ⇒ 底下的 `!` 一條都不生效；樣板檔看起來有效只因為早就被追蹤。改 `/SenateData/*` ＋逐層放行。驗 ignore 要帶 `--no-index`（不帶時已追蹤的檔一律不回報）。
2. `Buffer.BlockCopy` 不收 `Half[]`（不是 primitive）⇒ 走 `MemoryMarshal.Cast/AsBytes`。炸在嵌完 417 塊之後的寫檔那一步。
3. ServerSpawn 的「脫離行程樹」走 WMI ⇒ **子程序拿不到呼叫端的環境變數**；要傳的（HF_HOME、log 路徑）一律走參數。
另：新資料夾放在資料根底下（例 `_kb/`）要自帶 `.gitignore`，不然自動 commit 會把它列進未分類。
