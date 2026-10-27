---
id: pitfall_frame-time-profile-segmentation
topic: blockside-sim-step
title: 量每格時間別照 profile 切格：插計時的量測頁
type: pitfall
status: active
created_at: 2026-10-27
created_by: unknown
links: []
related_docs: []
---

量「每一格花多久」不要照 CPU profile 切格。遊戲很忙時兩格之間沒有閒置的取樣，前後幾格落在同一個最上層呼叫底下 ⇒ 被算成一格，格數與單格時間都偏高（2026-10-26 TASK-0817 第 3 格，前兩輪讀數因此不準，新舊比較仍可看）。
要準：在量測用的那份戰鬥頁直接插計時（只改量測頁、產品不動）—— erina session scratchpad/dist/tb_patch.py：townDraw 建的四圈（實例、地面、屋頂殼、燈）每格一筆 window.__tbL、townDistFrame 每次 > 1 ms 一筆 window.__tdL；load_trace.cjs 的 GO 跑完印 GO tb／GO td 分布、GO sig（實例數、每種模型雜湊、燈、townDump 地面雜湊）做新舊等價。
另：開著 profiler 本身會拖慢；量時間時關掉（load_trace 第 4 個參數 0）。城區頁偶爾按 D 叫不出來（GO 印 NOBTN）⇒ 那一輪作廢、補跑。燈、地面雜湊照「量完那一刻地形載到哪」而變，等價只比地形狀態一樣的兩輪。
