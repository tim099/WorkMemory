---
id: state_state-2026-08-12
topic: ucl-skill-install-sync
title: 三份鏡像追蹤不對稱已消失（全部追蹤、各 60 檔）；5 個 skill 退場完成；5 項 pending
type: state
status: active
created_at: 2026-08-12
created_by: apex-one
links: [persona-identity-layers]
related_docs: []
---

**現況（2026-08-12 by apex-one）**

## 三份鏡像的追蹤不對稱**已消失** —— 舊 state 的 pending 事實上被反方向解決了

2026-07-29 basecamp 記的 `pitfall_claude-skills-ucl-star-ignored`（`.gitignore:33` 擋
`.claude/skills/ucl-*/`）**現在不成立**。實測：

```bash
grep -n "skills" .gitignore          # → 零命中，完全沒有 skills 相關規則
git check-ignore -v .claude/skills/ucl-ding/SKILL.md   # → 未被 ignore
git ls-files .claude/skills | wc -l  # → 60
git ls-files .agents/skills | wc -l  # → 60
git ls-files .codex/skills | wc -l   # → 60
```

三份鏡像**全部追蹤、檔數對稱**。舊 pending「`.agents/skills/ucl-*` 要不要比照 `.claude` ignore」
已由現實選了方向 2 的極端版：**三份都不 ignore**（而且多了第三份 `.codex`）。
⚠ 我沒查到那條 ignore 規則是哪一筆 commit 拿掉的、也沒查是刻意還是順手 —— 若要追溯走
`git log -p --all -- .gitignore | grep -n "claude/skills"`。**這是本主題新的未解項。**

## 今天推進的：5 個 skill 退場 + 移除鏈修好

Skills~ 33 → 28（`ucl-persona-ding` / `ucl-self-constitution` / `ucl-session-handoff` /
`ucl-bartender` / `ucl-hook-setup`），三端已裝 29 → 25。三筆 UCL_Core commit：

- `4f9aa15` 工具：`--uninstall` 候選集 + `--force-remove-unmarked` + Matrix orphan 區
- `4abf7b1` persona-ding 退場（薄包裝，機制留 Ding_Protocol_Workflow Part 2）
- `6a7a10f` 四個退場（self-constitution 被 Constitution_Workflow 取代）

## pending（給接手的人）

1. **主專案鏡像層未 commit** —— 三份 `.claude`/`.agents`/`.codex` 的刪除與更新留在工作區。
   ⚠ 因為三份現在都追蹤，退場一個 skill 會產生**三倍**的 diff，這正是舊 state 擔心的「鏡像 sync commit」成本，
   現在變三倍。要不要重新考慮 ignore，是個有數字支撐的問題了。
2. `Bartender_Workflow.md` / `Hook_Setup_Workflow.md` 仍 `status: active`（實作 `Cmd_Bartender.cs` /
   `hook_validate_modified.py` 都還在）—— 「準備廢棄」是否落成 status，待 Tim。
3. 主專案 runtime `UCL_SkillConfigAsset/{ucl-self-constitution,ucl-session-handoff,ucl-hook-setup}.json`
   仍在（skill 已不存在 → 孤兒設定）。
4. `_manifest.json` 的 `ucl-watch-video` 死條目（manifest 28 vs 磁碟 27）——
   刻意未動：其 SkillConfigAsset Note 寫「需要時 --include 裝回」，語意是目錄應存在，
   目錄消失可能是意外，刪條目等於把意外追認成決定。
5. `en` / `ja` / `zh-Hans` 的 `UCL_AgentSkillManagerPage.md` 落後兩節（停在 86 行，
   還寫「Per-Agent × Per-Skill Matrix (TODO)」，而 Matrix 2026-07-27 已上線）。
