---
id: knowhow_pipeline
topic: sculpt-stormkaap
title: 施工骨架：sk_state 重播、apply_stamps、draft／apply／check 三件套
type: knowhow
status: active
created_at: 2026-10-14
created_by: unknown
links: []
related_docs: []
---

風暴角號的施工骨架（design/ 底下）：
- `sk_state.py` 照各階段 verify_*.json 的 stamp_order 重播現況（每刀驗：carve 守原色、非 overwrite 的貼原本要空）；一舷＋鏡像的件照 MIRRORED 重播兩份；外部模型的鍵帶 `_model`（occupancy 不算）。框全高 576 太大 ⇒ `state_after(d, stage, nz=200)`。
- `apply_stamps.py <design> <verify> <work> [--key k] [--from N] [--to M]`：一刀一刀下 stampvox／carvevox，讀回 placed，對不上就停並印從第幾項接。
- 每階段：draft_*.py（設計＋清單＋圖紙＋verify，重跑 md5 逐位元組相同）→ apply_*.py（建子作品、關聯、模型、固定點）→ check_*.py（結果帳 ＋ 從 plan 算的探針 ＋ 反向對照偏 3）＋ 引擎 stats structure=1 mirror=y:130。
- 探針要每一層照那一層的船殼半寬找牆（階段四：固定一欄的探針在 tumblehome、船尾收窄處報怪數）；開口量「刻開口的那一件」（階段五：量組裝件會先碰到玻璃、陽台）。
