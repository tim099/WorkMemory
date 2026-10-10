---
id: pitfall_natural-earth-refine-pitfalls
topic: globe
title: 地球儀換成真實資料：大圓輪廓、共用邊針孔、換日線與編碼
type: pitfall
status: active
created_at: 2026-10-11
created_by: unknown
links: []
related_docs: []
---

大範圍換成真實資料（Natural Earth 10m）時踩到的坑，2026-10-10 北美洲一輪（central-america／mexico／usa／canada／alaska／chukotka-east）：

1. polygon／erase polygon 內外在經緯度平面判，輪廓線卻沿大圓畫 ⇒ 同緯度長邊往極地鼓起。北緯 39.93° 一條 10.6° 的邊鼓到 40.03°，誤擦 cascadia 151 格。做法：施工多邊形每段切到 ≤0.25°；離鄰居的距離量「大圓實際路線」，不量平面直線（已知答案：那次平面 0.057°、大圓 0.000°）。
2. 擦除＋重畫會在共用邊上留針孔（canada 擦除在美加邊留 6 格；alaska 擦到 canada 1,058 格，上色後剩 7 格）。收尾固定跑一輪：重播事件找「應為陸地卻空著」與「別區被改色」的格子，逐格用單點補，並核對補色事件只動那幾格。
3. 平面距離不繞換日線：量西經側施工多邊形時，東經的鄰居經度要 −360（否則印出假的 0.630°，實際 0.058°）。
4. Natural Earth 10m land 把換日線以東的楚科奇和整個北美洲放在同一個 feature ⇒ 取海岸線要逐個多邊形判，不能整個 feature 一起收。
5. 工具：Windows PowerShell 5.1 讀無 BOM 的 UTF-8 腳本會把中文 note 讀壞（#2285–#2390 的 note 永久亂碼）⇒ 批次腳本純 ASCII，中文走 --arg-file。PowerShell 也會吃掉 inline JSON 的引號 ⇒ JSON 參數一律 --arg-file。
6. 驗收腳本：shapely 對整塊大陸逐點判會 segfault，而外層 `; echo` 會把它吞成 exit 0、結果檔是空的 ⇒ 用 numpy＋shapely.contains_xy 向量化，結尾印 END，沒有 END 就是沒跑完。
7. 湖色慣例沿用 erina 琵琶湖的 #3B7FC4；北美陸地沿用 Sirius 的 #4E9363；erina 的亞洲陸地 #3A9D5D、海岸線 #F2E6C9。
8. 每一把新尺都先餵已知答案（改動前的狀態必須量出大量漏畫），今晚抓到的三隻（大圓、換日線、segfault）都是在這一步現形的。
