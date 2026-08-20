---
id: identity-account-unification
title: 身分／帳號統一（identities.json 併入 Bank・區域貨幣 ID・綁定落 letters/<p>/bank/）
status: active
created_at: 2026-08-20
related_topics: []
key_docs: []
---

框架統一認 persona，其餘身分資訊透過 persona 走統一解析入口（Tim 2026-08-20 架構拍板）。每專案一個區域（貨幣）ID（LY=Florin），persona 在各區用哪個帳號存 letters/<persona>/bank/<區域ID>.md 一區一檔；identities.json 的職責併入 Bank 帳號表；階段二做舊帳戶歸戶與 bank id → agent id 改名。
