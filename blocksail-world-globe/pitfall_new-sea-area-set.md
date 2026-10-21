---
id: pitfall_new-sea-area-set
topic: blocksail-world-globe
title: 新的一片海域另開一套（--set）分開抽：火山島、母岩分兩半、碎片、海洋分區
type: pitfall
status: active
created_at: 2026-10-21
created_by: kotoko
links: []
related_docs: [docs/WORLD_FORMAT.md, docs/SPECIES.md, docs/WORLD_TOOLS.md]
---

新的一片海域（跟向風海峽不接壤）要畫分區、氣候、土壤 ⇒ 兩支抽取工具的 `SETS` 另開一套、`--set <名>` 分開算，舊檔一位元組都不動（小安地列斯 2026-10-21，TASK-0778）。改完一定先跑預設那一套、`sha1sum` 對 regions-windward 與 8 份 nature-*.js。

坑（都是量到才知道的）：
- 框碰到別國的本土（委內瑞拉）⇒ regions 照國家篩（`countries`）；nature 的陸地只算落在分區 2 km 內的格（`land_in_regions`），不然本土的格往海裡長、蓋到島那一區。
- SoilGrids 在小安地列斯幾乎沒給 Andosols ⇒ 火山島全落成石灰岩。母岩表的值寫 `(土壤, 說明)` 給年輕火山島「火山土」；一個分區裡母岩分兩半（瓜德羅普、安提瓜、巴貝多…）用 `BEDROCK` 母岩框。平原那一步預設不蓋火山土（保護 WRB 的 Andosols）⇒ 母岩表給的火山土會擋掉平原，這一套 `plains_keep` 只留沼澤、沙土。
- regions_extract 把「小於 0.5 km² 的碎片」一律當小島丟 ⇒ 小島教區簡化完的碎片也被丟，尼維斯峰頂留下沒有分區的陸地。`merge_small`：跟別區共用邊的碎片併給鄰居（只開給新的一套）。簡化改細反而更多窄縫提醒（300 m 6 條、100 m 24 條）。
- 窄縫提醒不等於縫：逐對量 `distance`、共用邊長、聯集有沒有洞；三區交會的尖角會誤報。
- 海裡的物種照海洋分區（caribbean／atlantic／deep）走 ⇒ 新海域沒有海洋分區 ＝ 一條魚都沒有、捕魚場蓋不了。小安地列斯補了 `sea-lesser.js` 兩區（沿島弧切）。
- 物種出處：POWO 擋 Cloudflare ⇒ 同一份 Kew WCVP 從 GBIF API 拿（資料集 f382f0ce-323a-4091-bb9f-add557f3a9a2）；WCVP 只分到背風、向風（含巴貝多）、千里達與托巴哥。
正本：WORLD_FORMAT §19「小安地列斯」、SPECIES §5.1、WORLD_TOOLS §5／5.1。資料在 kotoko 暫存區 `nature_lesser`（Köppen 全球、wrb_lesser.tif、ph_lesser.tif）。
