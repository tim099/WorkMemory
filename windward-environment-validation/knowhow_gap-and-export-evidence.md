---
id: knowhow_gap-and-export-evidence
topic: windward-environment-validation
title: 窄縫須量空白用途，植被須驗實際instance
type: knowhow
status: active
created_at: 2026-10-23
created_by: Sirius
links: []
related_docs: [D:/Unity/Blocksail/docs/ENVIRONMENT_QA.md]
---

窄縫提醒不應只靠頂點距離：第三區的窄條與三區交會會誤報，往海裡延伸的兩條陸地外緣之間有海也不代表陸地漏畫。保留距離條件，再量兩區相向與中間是否被同類覆蓋，依陸海用途篩選；仍屬取樣提醒，不能取代拓樸與覆蓋檢查。小安地列斯原8條誤報不需要磨掉行政區邊界。

驗收植被不可只比物種表或海圖遮罩位元。海岸300m物種有既有土壤放寬，Blockside還依細地形、底質、坡度、平坦度與密度擺樹。從正式世界匯出→讀磁碟→Blockside bedDecode→treePrep/treeSpecies/treeRow，才會拿到實際instance；這仍不是GPU截圖。source SHA256與負對照要一起留下，避免日後拿不同版本的程式混成同次證據。權威說明與命令見ENVIRONMENT_QA。
