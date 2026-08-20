---
id: state_ly-port-done
topic: tavern-web-reader
title: LY branch 移植完成（單一 commit fd0c78f01）
type: state
status: superseded
created_at: 2026-08-20
created_by: basecamp
links: [tavern-web-reader/state_ly-site-persona9999]
related_docs: [AgentCommands/ChatTavern/index.html, AgentCommands/.github/workflows/pages.yml, commit:fd0c78f01]
---

酒館網頁閱讀頁已從 main 移植到 LY branch，單一 squash commit `fd0c78f01`（四檔：ChatTavern/index.html／.github/workflows/pages.yml／.nojekyll／README.md），便於後續再移植到其他 branch/repo。

已驗（LY 資料實跑）：本機 `python -m http.server 8765`（在 ChatTavern/ 底下）開頁，最新 30 則載入、_seq.txt 對齊、日期下拉 68 天全載入、跨日接續檢查通過。

未做／邊界：
- pages.yml 觸發分支保留 `branches: [main]` —— LY push 是惰性的，不會搶走 Pages 部署（一個 repo 只有一個 Pages 站台，目前由 main 佔用）。要讓 LY 上線得改觸發分支＋repo Settings→Pages 切 GitHub Actions，且會覆蓋 main 的站台內容。
- 頁面前提是 seq 連續無缺號；斷點時 loadIndex() 會明講不會默默少給。
- file:// 直開不行（fetch 撞 CORS），一定要 http。
- 尚未 push（Tim 手動），父層（LY 主專案）pointer 未 bump。
