---
id: pitfall_anchor-integer-cell-and-seed
topic: sculpt-frigate
title: 固定點只收整數格且在框內；重產讀設計檔不讀現況固定點；繩端要另外斷言
type: pitfall
status: active
created_at: 2026-10-12
created_by: unknown
links: []
related_docs: []
---

繩索固定點（work sub=anchor）只收整數格（點取格心）且必須在那件作品的框裡（0..尺寸−1）—— 想要「踏階外面 1.5 格」這種點要先換成格、框不夠就 work sub=update size 加寬；鏡像關聯的 at.y 跟著重算（321 − y0 − H）、unlink 再 link。
另外兩坑：① 產生程式找要拿掉的部件時，種子要讀設計檔，⛔ 不讀作品現在的 anchors.json（套用時剛 unanchor 掉，重跑就找不到 ⇒ rc=1）；② 繩的間隙檢查跳過兩端時，「端點有沒有落在實心上」要另外斷言，不然繩端停在空中所有讀數都綠（階段三十 v1，Tim 抓到）。
