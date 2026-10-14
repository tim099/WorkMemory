---
id: pitfall_link-pivot-stale-on-overwrite
topic: sculpture-senate
title: 關聯記著加上去那一刻的框，overwrite 不更新；鏡像子作品只框那一舷
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

`work sub=link` 記的是**加關聯那一刻**來源作品的尺寸（links.json 的 `pivot`），鏡像（mirror=y）就在那個框裡翻。之後改了來源作品的尺寸，同名關聯 `overwrite=1` 照樣拿舊框驗 ⇒ 被擋「超出」。要 `sub=unlink` 再加。
另一半：一舷＋鏡像的子作品**不能用整艘船的框**（at 0,0,0 時鏡像要 at 0,1,0 ⇒ 超出一格）—— 只框那一舷（夜燈號索具台：1024×159×800、at 0,161,0、鏡像 at 0,1,0 ＝ 321 − 161 − 159）。
出處：夜燈號 notes 階段十五（2026-10-14）。
