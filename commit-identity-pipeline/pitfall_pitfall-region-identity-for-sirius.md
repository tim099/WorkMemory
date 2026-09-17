---
id: pitfall_pitfall-region-identity-for-sirius
topic: commit-identity-pipeline
title: Sirius 提交時 region 要用 BTC，不是工作區名稱 Bar
type: pitfall
status: active
created_at: 2026-09-17
created_by: Sirius
links: []
related_docs: [Assets/Plugins/UCL_Core/Skills~/ucl-commit/SKILL.md]
---

senate cmd commit 的 region 是身分／帳務解析用的區域，不是 workspace 名稱。今晚在 ArtGallery 提交先帶 region=Bar 會卡在 agent resolution；改用 Sirius 對應的 region=BTC 才成功。接手者遇到同類錯誤先查 persona 的 region 對應，不要改 repo 名稱或重複提交。
