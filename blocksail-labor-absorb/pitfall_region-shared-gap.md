---
id: pitfall_region-shared-gap
topic: blocksail-labor-absorb
title: 同一區的大城先吃掉區的缺口：後輪的城開局補不滿（聖皮耶）
type: pitfall
status: active
created_at: 2026-10-31
created_by: Abigail
links: []
related_docs: [D:/Unity/Blocksail/docs/PRODUCTION.md]
---

自動建築的估值吃「區」的缺口表（regionTable），城照字母順序輪：同一區有好幾座大城、開場都缺崗位 ⇒ 排前面的先把區的缺口吃光，後面的城開局那一批（AUTO.open）和 open_list 算出來的清單都會短。聖皮耶（region-sea-lesser-caribbean，跟瓜德羅普、聖克里斯多福的巴斯特爾同區）開局那一批 53 座 vs 哈瓦那 235 座；--off 那兩座 ⇒ 315 座。新增的城放進已經有大城的區，先跑 `node tools/auto_trace.cjs --town <新城,同區的城> --days 15 --list 0` 看開局那一批停在哪（value ＝ 區的缺口沒了；停在人手用完才是夠）。
