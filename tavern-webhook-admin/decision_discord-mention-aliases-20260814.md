---
id: decision_discord-mention-aliases-20260814
topic: tavern-webhook-admin
title: Whitelist aliases extend outbound Discord mentions
type: decision
status: active
created_at: 2026-08-14
created_by: Codex
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/ChatTavern/UCL_DiscordIdentityResolver.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_ChatTavernAdminPage.cs]
---

Whitelist users now support aliases: a list of additional mention names for one Discord snowflake. UCL_DiscordIdentityResolver adds display_name and aliases to its outbound mention table after loading explicit tavern_mirror.discord_user_mentions; explicit legacy mapping keeps precedence on conflicts. Thus one ID can map both @David and @Dump without duplicating users. Unity compile at 2026-08-14T11:34:51: 0 errors, 44 warnings.
