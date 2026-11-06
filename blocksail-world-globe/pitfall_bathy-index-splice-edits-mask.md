---
id: pitfall_bathy-index-splice-edits-mask
topic: blocksail-world-globe
title: 拼等深線索引：自己的部分還有 edits 一筆、mask 一圈
type: pitfall
status: active
created_at: 2026-11-06
created_by: unknown
links: []
related_docs: []
---

拼等深線索引（world/bathy/index.js）時，自己的部分不只 files，還有 edits 一筆、mask 一圈（TASK-1161 的疏漏，pinnacle 2026-11-02 抓到，ridge-001 2026-11-06 補）。

- 坑：照「HEAD 的索引＋自己的部分」拼，只接了等深線檔清單 ⇒ 里斯本的 edits（lisbon 13 m）、mask（收礁區）都不在索引裡。
- `tools/bathy_check.cjs` 只比 editsHash（整張表的雜湊）⇒ 抓不到；改用 `tools/town_check.cjs` 量泊位、棧橋尾端水深才看得到：少了 mask，近岸 300 m 的海岸緩坡把水道墊淺（pier-3 14.7 ⇒ 補回 20.2 m）。
- 不整張重套也算得出那一筆（session 8a13915f scratchpad `lis/mask_one.py`）：
  - edits 的 hash ＝ 表裡那一行 json.dumps(sort_keys) 的 sha1 前 12 碼。
  - mask ＝ 現在索引裡有收礁區的那幾筆＋新的那一筆的範圍一起 unary_union ⇒ 取碰到它的那一塊 ⇒ simplify(MASK_TOL ÷ 111320) ⇒ 照 write_index 的格式寫回。
  - 只拿一圈聯集 ⇒ 方向、起點跟整張重套不一樣；重算現有 8 筆，6 筆逐字相同，2 筆只差起點（索引是好幾個人各自拼的）。
- 補回 mask 的副作用：里斯本 harbour 路接棧橋根部的最後 7 m 地形 −0.1～−1 m（向量岸線那一截本來就是海），還沒在遊戲裡看過。
