---
id: state_ly-site-persona9999
topic: tavern-web-reader
title: LY 站台落戶 Persona9999/AgentCommands（fd0c78f01＋ab4becfa4）
type: state
status: superseded
created_at: 2026-08-20
created_by: basecamp
links: [tavern-web-reader/state_ly-port-done, tavern-web-reader/state_ly-msgindex-unignored]
related_docs: [AgentCommands/ChatTavern/index.html, AgentCommands/.github/workflows/pages.yml, commit:fd0c78f01]
---

LY branch 兩筆 commit 收束（AgentCommands submodule，皆未 push、父層 pointer 未 bump）：

1. `fd0c78f01` —— 從 main 移植閱讀頁四檔（index.html／pages.yml／.nojekyll／README），本機 http.server 對 LY 資料實跑驗過（68 天索引、跨日接續 OK）。
2. `ab4becfa4` —— Tim 拍板 LY 站台落戶 **Persona9999/AgentCommands**（remote `github.Persona9999`）：
   - pages.yml 觸發分支 main→LY；job 加 `if: github.repository == 'Persona9999/AgentCommands'` 守衛（LY 被推到 tim099/Valhalla 時不得蓋掉對面站台）。
   - configure-pages 前移拿 base_url；入口頁 repo 名 `__REPO__` 佔位＋GITHUB_REPOSITORY sed 代入（單引號 heredoc 不過 shell）；佔位殘留檢查用 if（bash -e 下 `grep && exit` 好情況會弄死整步）。
   - README 漂移探針：base_url grep README，對不上發 Actions warning（不 fail）。
   - README 連結 → https://persona9999.github.io/AgentCommands/ChatTavern/。

啟用前置（等 Tim）：Persona9999/AgentCommands Settings→Pages→Source 切 GitHub Actions；push LY 後第一次用 workflow_dispatch 跑。
Actions 上尚未實跑過 —— 本機只驗了 YAML parse 與 sed 模擬，workflow 全鏈路要等第一次 dispatch 才有讀數。
