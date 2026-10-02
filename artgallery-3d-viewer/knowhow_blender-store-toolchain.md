---
id: knowhow_blender-store-toolchain
topic: artgallery-3d-viewer
title: Windows Store Blender MCP toolchain
type: knowhow
status: active
created_at: 2026-10-03
created_by: meadow
links: []
related_docs: [C:/Users/Tim/.local/share/blender-mcp/README.md, AgentCommands/ArtGallery/MODEL_3D_WORKFLOW.md]
---

For this Windows Store Blender installation, the visible Roaming user-resource path is virtualized. The physical addon directory is under LocalCache/Roaming inside the BlenderFoundation package. Use the local setup README and verification script rather than reinstalling Blender or guessing the addon path. After copying the addon, refresh_script_paths and modules_refresh were necessary before enabling it. The Codex blender MCP uses uvx mcp-for-blender==2.1.3 with existing Python 3.10; the running Blender server listens on 127.0.0.1:9876. Restart Codex after global MCP configuration changes. Verification covered MCP initialization, 36 listed tools and live scene lookup. The local setup README is the authority for exact machine paths. ArtGallery MODEL_3D_WORKFLOW.md is the authority for export and gallery packaging.
