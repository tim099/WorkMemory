---
id: pitfall_med-fine-extraction
topic: blocksail-world-globe
title: 地中海細區：疊兩套等高線、窄水道被封、原始圖磚在哪
type: pitfall
status: active
created_at: 2026-10-21
created_by: kotoko
links: []
related_docs: []
---

地中海細區（2026-10-21 合進 Blocksail main 1b81309／9b15d9a；正本 WORLD_FORMAT §10.2）—— 踩過的坑，下次加細區先看：

1. 原始圖磚不進版控、別人重抽要有它：全套在 kotoko 暫存區 `C:/Users/Tim/AppData/Local/Temp/claude/D--Unity-Senate/0dd92658-7f7d-49a5-86b0-6cec99bd89eb/scratchpad/dl_med`（加勒比海 2,452＋地中海 1,718 張 z10、博斯普魯斯 z12 72 張、NE）。少了地中海那批 ⇒ 工具現在 SystemExit（以前安靜跳過、整片退回 NE）。
2. 等高線是「每一框各抽各的」、海岸線是「一框一框依序換」⇒ 框重疊的地方海岸線沒事、等高線會疊兩套。⛔ 切重疊小框；巢狀的細框（博斯普魯斯）用 `_cut` 把外框的等高線挖掉。驗法：數「碰到內框的等高線」改前改後（52 → 142 是疊了）、六座城規劃雜湊。
3. neGate 的兩個放寬（grow 往海裡多長、hill 山附近照 DEM）在窄水道會把海峽與灣口封死（薩拉米斯、卡洛尼、格拉、米洛斯；達達尼爾 600 m）。地中海用 100 m／0。判準：先在單一框試三組、數「被封成湖的灣」。
4. 量海峽寬用「瓶頸寬」（a→b 能通過的最大圓，距離轉換＋二分＋連通），⛔ 不用 fairway 的 2×最小離岸（路會貼岬角走、低估）也 ⛔ 不用最短弦（掉進小灣）。工具在暫存區 `tools_med/med_bottleneck.py`。
5. 圖磚的海不一定有水深：同一片海有 103 張是 0 m（亞得里亞海南段）⇒ 一張一張判（flat_sea）。缺圖磚的內陸要平滑補（金字塔、只拿陸地像素）——最近值會拉直紋、連海平均會壓成 1 m 平台。
6. 別人同時在整份重抽（kiara 一晚兩次）⇒ 每次合 main 之後都要照新的 main 重抽；合之前查 `git log <base>..main -- world/index.js world/tiles tools/world_extract.py`。兩個抽取同時跑一個 17 GB，48 GB 的機器只能一次一個。
7. 驗收清單：對照組（只給舊框 ⇒ world/ 不變）、平行重抽逐位元組相同、在結果上重抽 0 新號、切開再拼回 0 錯、航線（地球儀頁 BSW.route，node 沒有 canvas 跑不了）、areas_check 跟 base 逐字比、town_plan 雜湊（跟同一個 base 的乾淨工作區比，⛔ 跟移動中的 main 比）。
