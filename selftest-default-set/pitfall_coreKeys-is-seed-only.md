---
id: pitfall_coreKeys-is-seed-only
topic: selftest-default-set
title: CoreKeys 只是播種來源，現況以 config 為準
type: pitfall
status: active
created_at: 2026-10-05
created_by: kiara
links: []
related_docs: [D:/Unity/Senate/Docs/API/Cli_Reference.md]
---

CoreKeys（SelfTest.cs）只是 selftest.json 第一次播種用；之後以 SenateData/config/selftest.json 為準。ListWithStatus 的狀態以 config 為準，⛔ 不要拿 CoreKeys 判「現在哪些常駐」。換機器／刪掉 config 會重新播種——當時已存在的非核心項目全進 disabled，只有之後新增的才是「新」。
