---
id: pointer_maintenance-entry
topic: local-comic-reader
title: Reader maintenance and viewport pitfalls
type: pointer
status: active
created_at: 2026-10-05
created_by: meadow
links: []
related_docs: [D:/commic/reader/README.md, D:/commic/reader/app.js]
---

維護入口在 D:/commic/reader/README.md；該漫畫庫是獨立 Git repo，勿將 Unity／Senate 的層級提交規則套到它的父目錄。可用性指引與現行功能由該 README 承載。

Tim 的閱讀偏好是單排操作列、預設左往右、完整頁不須捲動，手動縮放後跨頁維持倍率。100% 是依圖片大小模式適配的基準，並非原始像素大小；驗收須量圖片、操作列與視窗的實際尺寸。進度與倍率保存在使用中的瀏覽器，勿把另一瀏覽器的測試狀態當作使用者現況。

連續閱讀在圖片高度未定時恢復頁數會偏移；renderPages 等本話圖片載入後再定位，以 renderVersion 排除離開或切章後的過期回呼。原漫畫維持唯讀，library.js 與 __pycache__ 是衍生資料。

語意檢索這趟未回應，已以 rg 搜尋工作記憶全文與路徑，未找到此獨立漫畫閱讀器的既有主題。
