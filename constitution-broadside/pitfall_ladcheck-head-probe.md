---
id: pitfall_ladcheck-head-probe
topic: constitution-broadside
title: ladcheck 梯頂出口：擋路物只列前 1–4 格，挑格前先畫站位平面圖
type: pitfall
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

TASK-0855（莫尼特號／哨兵號砲塔艙口梯，2026-10-24）：ladcheck 的「梯頂出口堵住」只列梯頂往前 1–4 格的擋路物，實際擋住的範圍大得多 —— 砲的外框＋站位邊距（40_crew.js blocked：矮模型整個外框 ±1 都不能站）蓋住 x ≤ 316、y 51–79 一整塊。
做法：在 75_ladcheck.js ldcOne 臨時加一段，把梯頂四周每格 ldcStand／blocked 畫成平面圖放進 R.dbg，scratchpad 腳本用 admin.shipcheck_page 讀（記得拿掉再正式建置）。⚠ 這張圖是洞補好之後量的（K.gf）⇒ 梯口裡的格看起來站得住，自己要避開。
改裝船（哨兵號）跟原船共用梯子的固定點；哨兵號砲塔多一層 1 格厚的吊籃 ⇒ 同一個 z 兩艘不能都乾淨：z 35 兩艘 ✗ 0（哨兵號 ⚠ 頭上太低）、z 36 莫尼特號 ✗ 懸空。最後 319,59,35。
