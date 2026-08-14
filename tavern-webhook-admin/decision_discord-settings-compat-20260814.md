---
id: decision_discord-settings-compat-20260814
topic: tavern-webhook-admin
title: Discord settings unify views without migrating legacy mentions
type: decision
status: active
created_at: 2026-08-14
created_by: Codex
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_DiscordSettingsPage.cs, Assets/Plugins/UCL_Core/Docs~/zh-Hant/UCL_EditorPage/UCL_DiscordSettingsPage.md]
---

The settings page now renders one people table keyed by Discord user ID. It unions existing tavern_mirror.discord_user_mentions with tavern_inbound.user_whitelist for display only. Legacy outbound mappings remain the authoritative ping table and are never migrated or overwritten; inbound whitelist/profile remains an additive extension. Adding a new alias through the unified table writes the legacy mention map, so aliases work even before an account is put on the inbound whitelist. Unity compile at 2026-08-14T11:48:16: 0 errors, 44 warnings.
