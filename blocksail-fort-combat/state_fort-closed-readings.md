---
id: state_fort-closed-readings
topic: blocksail-fort-combat
title: 結單時的狀態：修繕驗法、修繕天數讀數、鐵砲缺口沒動
type: state
status: active
created_at: 2026-10-31
created_by: summit
links: [blocksail-fort-combat/pitfall_fort-open-gaps]
related_docs: []
---

結單時（2026-10-31，Tim「照讀數結單」：0953／0954／0956／1093 的「Tim 進遊戲看」那幾格沒等他打，改以讀數為準）留下的狀態：

- 修繕驗法：`tools/fort_check.cjs`（Blocksail）。它照 id 挑第一座人口 ≥ 500 的城 ⇒ 新加的城會換掉它挑到的城（10-31 挑到巴斯特爾：沒磚、沒石灰、島上燒不出來 ⇒ 6 ✗）。3ca6a553 起修繕段放磚、木板各 60 桶進那座城的市場、買砲家族挑倉庫放得下的；再 ✗ 先用 `FT=<城 id>` 釘到別座城跑，分清是檢查還是遊戲。
- 修繕要多久（起始檔照跑、不放料、沒有 AI 船隊貿易 ⇒ 偏悲觀）：只打牆一成 ⇒ 哈瓦那 20 天、皇家港 87、聖地牙哥 94、托爾圖加一年沒修好（沒磚）。打掉 3 門砲（7.5 t）⇒ 四座都一年修不完：堡壘自己囤 2 t、聖地牙哥一年只買到 7.3 t。
- 鐵砲缺口沒動：照 Tim 對加勒比缺貨定的方向，等歐洲港與航線（記憶 blocksail-europe-ports-for-imports），不另外變出砲。之後有人調平衡或接歐洲港，先重量上面這張表。
- 實驗場 10-31 重開（Blockside dev e703b47、`?mode=pve&blue=constitution&red=fort_en&seed=7&debug=1&headless=1`）：第 105 秒挖掉 27,085 格、9 門剩 5、死 9 傷 11、還擊 6 發；265 秒沒失守 —— 憲法號 AI 開到 600 m 外再回來。藏著的分頁每次 `__game.run` ≤ 40 秒，不然逾時跳到 data:。
- pitfall_fort-open-gaps 那幾條（砲組不跨半邊、碰撞、平衡〔佔位〕材質）結單時都還在。
