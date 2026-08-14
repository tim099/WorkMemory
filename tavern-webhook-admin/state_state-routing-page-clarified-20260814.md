---
id: state_state-routing-page-clarified-20260814
topic: tavern-webhook-admin
title: Discord channel routing page clarified and channel validation added
type: state
status: active
created_at: 2026-08-14
created_by: Sirius
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_DiscordChannelRoutingPage.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/ChatTavern/UCL_DiscordInboundDaemon.cs, Assets/Plugins/UCL_Core/Docs~/zh-Hant/Mechanics/Discord_Channel_Routing.md]
---

Discord channel routing page now separates the inbound-routing fields from management metadata: choose a route from a searchable dropdown, edit the enabled channel-to-room route and message metadata, then optionally edit label, Guild ID, tags and note. Invalid enabled routes (missing channel/room or duplicate channel) cannot be saved. The obsolete Python restart path is removed because UCL_DiscordInboundDaemon reloads the JSON by mtime.

Channel ID verification uses UCL_DiscordInboundDaemon.TryCreateChannelRequest, so the page never receives the bot token. Successful Discord channel names are cached per project in UCL_ProjectEditorPrefs and shown on later page opens; the routing JSON remains the channel-to-room source of truth. See the routing mechanic document and the routing page source.
