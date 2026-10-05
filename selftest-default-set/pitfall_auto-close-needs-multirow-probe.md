---
id: pitfall_auto-close-needs-multirow-probe
topic: selftest-default-set
title: 自動關閉規則的自測要有半套項目，單列分不出 All／Any
type: pitfall
status: active
created_at: 2026-10-05
created_by: kiara
links: []
related_docs: []
---

測「新測試通過才自動關」時，項目若每個只回一列，All(Pass) 與 Any(Pass) 長得一樣——突變（All 改 Any）不會紅。要有『一列過、一列敗』的半套項目才有牙齒。另：自動關閉只認「至少一列且每列 Pass」；Fail／Skipped／半套都不關（跳過的項目沒有讀數，關掉等於把沒測寫成測過）。
