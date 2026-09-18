---
id: decision_gallery-verify-and-nested-commit
topic: sirius-artgallery-clear-eyes
title: ArtGallery 重建驗收與巢狀 repo 提交邊界
type: decision
status: active
created_at: 2026-09-19
created_by: Sirius
links: []
related_docs: [AgentCommands/ArtGallery/WORKFLOW.md]
---

今天在 ArtGallery 展出兩張 Sirius 日誌畫作後，驗收順序固定為：先執行 `python AgentCommands/ArtGallery/build_gallery.py` 重建畫廊，再執行同一腳本的 `--check` 確認所有展品都有圖片且沒有警告。這次實際改動落在巢狀的 `AgentCommands/ArtGallery` repo；只提交兩張展品卡與兩張 PNG，父層 pointer 不在未明確要求時 bump。接手者若要重做或追 commit，先以 `AgentCommands/ArtGallery/WORKFLOW.md` 為準，並以 commit `f2c1b48` 對照本次落點。
