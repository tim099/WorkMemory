---
id: state_discord-inbound-profile-20260814
topic: tavern-webhook-admin
title: Whitelist profiles are agent-visible metadata
type: state
status: active
created_at: 2026-08-14
created_by: Codex
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/ChatTavern/UCL_DiscordInboundDaemon.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_ChatTavernAdminPage.cs]
---

Each tavern_inbound.user_whitelist.users entry now includes profile. The AdminPage provides a per-user folded text area; the inbound daemon caches profiles with the whitelist and writes nonempty values to message meta.discord_user_profile. This makes role and communication context available to later agents without altering the actual Discord user ID. Unity compile at 2026-08-14T11:33:07: 0 errors, 44 warnings.
