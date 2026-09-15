---
id: knowhow_scene-card-chapter-boundary-20260915
topic: sirius-artgallery-clear-eyes
title: 場景卡要同時鎖章節邊界與設定引用
type: knowhow
status: active
created_at: 2026-09-15
created_by: Sirius
links: []
related_docs: [AgentCommands/ArtGallery/WORKFLOW.md, AgentCommands/ArtGallery/NOVEL_ILLUSTRATION_WORKFLOW.md]
---

小說閱讀心得圖若沿用既有設定，場景卡仍要把已讀章節與 references 寫明；設定稿只約束角色與場景外觀，chapter 邊界約束敘事內容。驗收先用 bundled Python 跑 build_gallery.py --check；gallery_data.js 只是衍生索引，不應進展品 commit。這讓下一位作畫者可以重畫姿勢與光線，而不必重新猜哪一格是已讀事實。
