# 工作記憶索引 — sculpt-napoleon

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_boiler-material** — 鍋爐金屬質感用材質 boiler（不標 iron）
- **decision_power-stations-meta-only** — 動力件站位：模型只標站位、參數在遊戲端

## knowhow
- **knowhow_fps-oneway-reach-check** — 第一人稱：走得進去不等於走得回來（量法）
- **knowhow_furnish-as-deco-models** — 陳設做成外部模型、標 deco=1；擺放與出圖的做法
- **knowhow_propeller-shaft-binding** — 螺旋槳做成模型、軸的 aft 24 格內才接得上、鍋爐不標 iron
- **knowhow_readback-game-version** — 回讀遊戲裡匯進去的是哪一版（shipcheck 乾淨副本）

## pitfall
- **pitfall_game-x-11bit** — 遊戲格座標 x 上限（10→11 位元）
- **pitfall_material-needs-reimport** — 改 material／smooth 要整份 reimport，metasync 帶不過去
- **pitfall_split-leaves-broken-remainder** — 拆件之後剩下的件斷了（軸穿小齒輪）
