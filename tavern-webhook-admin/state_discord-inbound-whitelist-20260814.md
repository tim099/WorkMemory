---
id: state_discord-inbound-whitelist-20260814
topic: tavern-webhook-admin
title: Discord inbound user whitelist implemented
type: state
status: active
created_at: 2026-08-14
created_by: Codex
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/ChatTavern/UCL_DiscordInboundDaemon.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_ChatTavernAdminPage.cs, Assets/Plugins/UCL_Core/Docs~/zh-Hant/Mechanics/Discord_Channel_Routing.md, Assets/Plugins/UCL_Core/Docs~/zh-Hant/UCL_EditorPage/UCL_ChatTavernAdminPage.md]
---

UCL_ChatTavernAdminPage now manages tavern_inbound.user_whitelist: enabled plus user_id/display_name entries. UCL_DiscordInboundDaemon mtime-caches it, rejects non-listed human messages before attachment download when enabled, and uses display_name only as an explicit Tavern display override. Missing whitelist remains disabled for backward compatibility; enabled with an empty list denies all. Unity compile at 2026-08-14T11:30:21: 0 errors, 44 existing warnings.
