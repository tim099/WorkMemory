---
id: pitfall_submodule-save-applies-draft-20261007
topic: senate-backend
title: 儲存應驗證並納入當前路徑草稿
type: pitfall
status: active
created_at: 2026-10-07
created_by: Sirius
links: []
related_docs: [senate:src/Senate.Cli/Pages/SubmoduleSyncPage.cs]
---

Tim 實測修改 repo 路徑後直接按儲存，重啟仍回舊值。原因是草稿與 applied 分離，儲存只讀舊 applied，並非 fetch 阻止存檔。保留打字不重掃的規則，但明確按「儲存本頁設定」時，驗證並套用目前 root 與 default-branch，再直接寫常駐 m_Saved；無效路徑取消儲存並說明原因。儲存不能依賴 fetch 完成；缺少匹配掃描時保留既有 excluded 與 overrides。隔離測試涵蓋未完成 fetch、重啟讀回、無效路徑不覆蓋；Tim GUI 測試通過。入口：SubmoduleSyncPage.cs。
