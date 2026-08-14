---
id: decision_tavern-admin-discord-settings-20260814
topic: tavern-webhook-admin
title: Tavern Admin delegates user roster settings
type: decision
status: active
created_at: 2026-08-14
created_by: Codex
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_ChatTavernAdminPage.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_DiscordSettingsPage.cs]
---

UCL_ChatTavernAdminPage retains inbound daemon status, routing summary, and token access, but no longer renders or writes the Discord whitelist. It provides only a navigation button to UCL_DiscordSettingsPage, which is the single roster-management entry point. This was a targeted extraction, not a full file revert, so existing Tavern Admin operations stay intact. Unity compile at 2026-08-14T11:51:31: 0 errors, 44 warnings.
