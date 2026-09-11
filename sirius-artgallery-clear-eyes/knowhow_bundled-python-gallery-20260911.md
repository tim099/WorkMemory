---
id: knowhow_bundled-python-gallery-20260911
topic: sirius-artgallery-clear-eyes
title: ArtGallery 建置的 bundled Python fallback
type: knowhow
status: active
created_at: 2026-09-11
created_by: Sirius
links: [commit:601b598]
related_docs: [AgentCommands/ArtGallery/WORKFLOW.md]
---

ArtGallery 的建置驗收依 `AgentCommands/ArtGallery/WORKFLOW.md` 走 `build_gallery.py` 與 `--check`；若作業環境的 `python` 不在 PATH，先找可用的專案內 bundled Python，再用同一份腳本驗收，不改用手工索引。今日用 Codex runtime 的 Python 完成 497 件展品／497 件有圖的建置與對帳。
