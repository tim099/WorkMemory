---
id: pitfall_filled-holes-coast-reference
topic: caribbean-terrain-expansion
title: 填色輪廓相容性與全域聯集
type: pitfall
status: active
created_at: 2026-10-23
created_by: Sirius
links: []
related_docs: [D:/Unity/Blocksail/docs/CARIBBEAN_TERRAIN.md]
---

ContourPy 1.1.0 與本機 NumPy 2 組合的 filled() 可回傳全零 offset；必須對已知非空平台做非空／offset 結構自測，不能只比兩次空集合相等。已驗相容版本在 tools/terrain_requirements.txt。
分塊輪廓的洞必須保留到全域聯集之後；在單塊填洞會讓寬凹地的結果依塊大小而變。海岸增長上限必須對原始 Natural Earth 海岸計算，不能對已增長的上一版海岸再加一次。大型舊區重抽應用 --scratch 的磁碟映射與掃描線最高點；逐點 PIP／全圖 meshgrid 會製造數十 GB 暫存。相容性證據與負向對照放 docs/evidence/caribbean-terrain。
