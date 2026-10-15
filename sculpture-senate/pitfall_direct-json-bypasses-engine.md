---
id: pitfall_direct-json-bypasses-engine
topic: sculpture-senate
title: 造船產生程式直接寫 json 會繞過引擎檢查；重跑的兩個坑
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

炎羽號（TASK-0587）10-14 的產生程式直接寫組裝件的 `links.json`／`models.json`／`ropes.json`／`sails.json`，繞過 `work sub=link|model|rope|sail` 的檢查 —— 帆桁掛錯桅、繩長 0、舵模型 at 0,0,0 浮在船外，全部「寫得進去、讀得出來、broken 0」，而筆記寫了「完全相容」。
⇒ 一律走 CLI（v2 在 `kiara-phoenix-01m/design/v2/`）；舊的那六支已加 `sys.exit` 擋住。

重跑要逐位元組相同時的兩個坑：
- `sub=rope` 只要給了 from／to，引擎就當成「換端點」而擋下（即使字一模一樣）⇒ 已經有、兩端相同的繩不給 from／to；不同才加 `overwrite=1`。
- 不要先 `unrope` 全部再重建（created_at 會變）—— 只拿掉不在新清單裡的。

給遊戲之前另外要量的（引擎讀數管不到）：模型零碰撞要自己算（格心＋八角落不落在組裝件實心格，verify_models.py）；組裝件 6 連通元件數（浮著的甲板／桅頂平台／彈藥庫會在遊戲裡掉下去）；繩端是座標時，2 格內要有實心（Blockside ropecheck 照這個判浮空）。
