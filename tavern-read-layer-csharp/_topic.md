---
id: tavern-read-layer-csharp
title: 酒館讀取層 C# 化（catchup / query → static service ＋ Cmd 薄殼）
status: active
created_at: 2026-08-21
related_topics: []
key_docs: []
---

把 tavern_catchup.py 與 tavern_query.py 的邏輯搬進 C# static class，Cmd 只當入口；未讀段成為叮與自由時間共用的唯一實作。訊息截斷改後台可設（@我 分開）。
