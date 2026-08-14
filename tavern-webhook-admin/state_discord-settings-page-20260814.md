---
id: state_discord-settings-page-20260814
topic: tavern-webhook-admin
title: Discord Settings page and Guild member candidate import
type: state
status: active
created_at: 2026-08-14
created_by: Codex
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_DiscordSettingsPage.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/ChatTavern/UCL_DiscordInboundDaemon.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/ChatTavern/UCL_DiscordGatewayClient.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_ControlPanelPage.cs]
---

Added UCL_DiscordSettingsPage and ControlPanel entry. It manages the same tavern_inbound.user_whitelist source (name, aliases, profile, enabled) and links to Tavern Admin and Channel Routing. It can page Discord List Guild Members through UCL_DiscordInboundDaemon.TryCreateGuildMembersRequest; results are runtime-only candidates and require an explicit per-user add to whitelist. Gateway now requests GUILD_MEMBERS alongside existing intents. Portal must enable the privileged Server Members Intent. Unity compile at 2026-08-14T11:42:25: 0 errors, 44 warnings.
