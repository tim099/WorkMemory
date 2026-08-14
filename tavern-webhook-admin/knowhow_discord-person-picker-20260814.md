---
id: knowhow_discord-person-picker-20260814
topic: tavern-webhook-admin
title: Discord settings uses a searchable person picker
type: knowhow
status: active
created_at: 2026-08-14
created_by: Codex
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_DiscordSettingsPage.cs]
---

Unified Discord people management no longer renders every member editor at once. UCL_DiscordSettingsPage builds its ID-keyed union, presents labels through UCL_GUILayout.PopupSearchCache with a dedicated m_PickerDic, then renders operations only for the selected person. m_FoldDic remains separate for fold preference and m_PickerDic is cleared on data reload. Unity compile at 2026-08-14T11:49:38: 0 errors, 44 warnings.
