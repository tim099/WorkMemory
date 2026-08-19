---
id: pointer_doc-map
topic: persona-registry-retirement
title: 文件地圖：規格書＋接縫＋快照＋審計的落點
type: pointer
status: active
created_at: 2026-08-19
created_by: summit
links: [persona-letters-repo/state]
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/Plan/Plan_Persona_Registry_Retirement.md]
---

## key → 權威文件

- **本案唯一規格書**：`Assets/Plugins/UCL_Core/Docs~/zh-Hant/Plan/Plan_Persona_Registry_Retirement.md`
  —— §1 消費端盤點（calli 實掃 32 支）／§2 欄位判定／§4 分期＋Phase 0 已遷/未遷清單／
  §5 為什麼改名驗不出來（毒藥檔）／§8 Tim 拍板全集（8.1 bank 反向登記、8.2 profile/ 一欄一檔、
  8.3 欄位分家表、8.4 lazy migration、8.5 now_status＋presence、8.6 寫入接縫、8.7 A+B 解析單端）
- **讀取接縫**：C# `UCL_Core_Scripts/EditorCore/UCL_AgentCommands/AwakenInit/UCL_PersonaProfile.cs`
  ⇄ python `Tools~/AgentCommands/_lib/persona_profile.py`（三段 fallback：Cmd→快照→local-parse，
  非現場值帶 _source/_snapshot_at 標記）
- **Cmd**：`Cmd_PersonaProfile.cs`（op=refresh 刷快照／op=set 寫入，actor+reason 必填）
- **快照**：`AgentCommands/AwakenInit/_persona_profile_snapshot.json`（C# 只寫，gitignored）
- **寫入審計**：`AgentCommands/AwakenInit/_persona_write_audit.jsonl`（append-only，入版控）
- **presence 唯一掃描**：C# `UCL_ActivePersonaLocks.cs` ⇄ python `awakening.list_locks()`
- **連動備忘**：券錢包遷移 `Plan_Voucher_Wallet_Migration.md`（backlog，先 registry 後券）
