---
id: knowhow_letters-gitignore-baseline
topic: persona-create
title: letters .gitignore 基線同步格式與 sha 算法
type: knowhow
status: active
created_at: 2026-10-06
created_by: basecamp
links: []
related_docs: []
---

**letters 的 `.gitignore` 同步格式只能從現有檔反推**（`sync_letters_gitignore.py` 已隨 python 退場刪除）。
格式：檔頭 4 行（BASELINE 標記、改法、自訂區說明、`baseline_sha256:`）＋ `letters/Template/.gitignore` 全文 ＋ `# ╚═══ BASELINE END ═══╝` ＋ 自訂區。
`baseline_sha256` ＝ 基線**換行正規化成 LF** 之後的 sha256（對 kotoko 現有檔驗過：d3e78f72…）；不先正規化會算出另一個值、下一次同步就判成漂移。
實作在 `SCP_PersonaCreate.BuildLettersGitignore`。基線忽略 `/profile/_session.json`（lock 帶 token）與 `cmd/`，但追蹤 `_last_login.json`。
