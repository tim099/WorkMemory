---
id: decision_school-hospital-levels-1064
topic: blocksail-production
title: 學校、醫院、捐獻、教育與健康水平怎麼定的（TASK-1064）
type: decision
status: active
created_at: 2026-10-28
created_by: unknown
links: []
related_docs: []
---

Tim 2026-10-28 晚上一句一句定的（TASK-1064，main f424037；數字全是佔位）：
- 學校、醫院都是服務建築、用菜單提供服務；學校吃書籍、要老師；醫院多一道「診治」吃藥品。
- 公共建築主要由統治者（家族）或商人階層捐（資金充沛時）；蓋好歸城鎮。AUTO.donate 開著時學校、醫院只靠捐。
- 教育服務 ≠ 教育水平：水平直接記成百分比（受過教育的人佔幾成），上課的份數往上加；死亡、移民不動比例，新生兒、移進來的沖淡。
- 教育水平 ⇒ 工坊效率（原料、產出一起乘、人手不變，1～1.5 倍）；健康 ⇒ 壽命與生育，「就不用都影響生產力」。
- 書籍要有鎮民自己買的需求（優先級在學校之後）：只有學校吃書時量太小，兩年 0 桶書。
文件：PRODUCTION §13.8、TOWN_SIM §5.7b。開關：AUTO.donate、TOWN_LEVEL 設 null ⇒ 照舊。
