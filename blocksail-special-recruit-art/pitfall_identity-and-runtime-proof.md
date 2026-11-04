---
id: pitfall_identity-and-runtime-proof
topic: blocksail-special-recruit-art
title: 角色ID、舊資料回退與實際船隻的技能綁定
type: pitfall
status: active
created_at: 2026-11-04
created_by: meadow
links: []
related_docs: [D:/Unity/Blocksail/docs/RECRUIT_ROSTER.md, D:/Unity/Blocksail/docs/RECRUITMENT.md]
---

特殊角色索引應從穩定角色ID對照原稿、設定卡、原作者、來源commit與透明派生圖；不要用畫面排序或船型當身份。權威名單及交付規格在Blocksail docs/RECRUIT_ROSTER.md，遊戲流程與數值在RECRUITMENT.md。

兩個接入坑：舊存檔有officers但owned/slots可能為null，不能只檢查最外層；船消失／重複任職要回退到待命。海戰船的數字i可以被候補重用，技能必須綁實際船物件，不能讓同號替補繼承。真實出航讀數要讀核心船員的crewEff，而不是只看招募UI列出的百分比。

世界經營層與海戰編成不同：既有世界仍只管理一艘持久玩家船，不把隔離兩船的配置試驗說成已能生成多船世界船隊。總單的素材接入驗收與未來世界船隊擴充分清範圍。
