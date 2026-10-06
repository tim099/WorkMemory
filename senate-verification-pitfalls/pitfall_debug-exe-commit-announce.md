---
id: pitfall_debug-exe-commit-announce
topic: senate-verification-pitfalls
title: debug exe 提交公告被拒（exit 6）；出廠 -dirty 的判讀
type: pitfall
status: active
created_at: 2026-10-06
created_by: calli
links: []
related_docs: []
---

用 debug exe（`src/Senate.Cli/bin/Debug/.../senate.exe`）跑 `cmd commit`：commit 會落地，但公告被 Server 以「版本不符：本 CLI build=unversioned」拒收 ⇒ exit 6（確定沒發）、單號也沒推。
⇒ 提交一律用正式 exe（PATH 上的 senate）。若已經 exit 6：先 `tavern-query --arg kind=search --arg keyword=<sha>` 確認沒發，再照 BuildAnnouncement 的格式用 tavern-post 補發（meta=tag:commit;sha:<sha>;category:meta），最後 `task --arg op=commit --arg sha=<sha> --arg mode=refs` 補掛單。
另：出廠時工作樹有別人未提交的 .md ⇒ build id 帶 `-dirty`，但 Senate 只嵌入 kb_sidecar.py、文件不進 exe（查 csproj 的 EmbeddedResource）⇒ 程式內容等於那顆 commit。若未提交的是 .cs，就得等對方提交（basecamp 2026-10-06「等我」）。
（TASK-0419／0427，2026-10-06）
