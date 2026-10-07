---
id: decision_unity-keeps-only-project-features
topic: unity-senate-migration
title: Unity 只留 Unity 專案功能；關場在 Senate 就地做
type: decision
status: active
created_at: 2026-10-07
created_by: unknown
links: []
related_docs: []
---

Tim 2026-10-07 拍板（TASK-0324 結單時）：

- **Unity 端只留 Unity 專案本身的功能**：資產／建置／Hierarchy／Recompile／Invoke／TypeInspect／Schema 那 15 支指令、ucmd 執行器（Runner／Queue／Watcher／Registry…）、`UCL_CompileErrorTracker`（寫編譯狀態檔，Recompile 與編譯閘都靠它）、`UCL_EditorHeartbeat`。其餘遷到 Senate 或重做。
- **按功能刪，不必先查使用端**（酒館已整個在 Senate）。殘留功能 → TASK-0459；殘留文件與 skill → TASK-0458。
- `UCL_DocSearchPage`（文件搜尋）**保留**；`UCL_SecretManagerPage`／`UCL_SecretDaemon`（密鑰）**刪**；`UCL_LLMModelAdminPage` 那族**廢棄**。
- **觀影重做、不遷移**（TASK-0450，影音管理頁 TASK-0392 併入）。Unity 觀影已刪（TASK-0449）。
- **關場一律在 Senate 就地做、不結算**（TASK-0448）：`SCP_ActivitySessionStore.CloseVerified`＝翻三欄＋回讀。委派 Editor 的關場閘已整套拿掉；Senate 端委派到 Editor 的只剩 `unity-compile` → `Recompile`。
- 活體測 session／晚安用 **Template** persona（Tim 指定）。

⚠ 兩個坑：
- Unity 側 `hours` 參數吃正整數，做不出「一分鐘後過期」的殘留場 ⇒ 實測時手改 Template 那顆的 `end_ts`；改的時刻要留餘裕（10-07 改成 22 秒後，跑測試時還沒過期，量到的是「進行中被擋」）。
- 刪 Unity 檔一律 `git rm`（含 .meta 與資料夾 .meta）；shell 整檔覆寫會被權限擋，改程式用 Edit。
