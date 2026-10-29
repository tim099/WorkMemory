# 工作記憶索引 — blocksail-labor-absorb

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_build-pace-3** — 建築名額 7 ⇒ 3 天、奢侈品打五折、教堂留料（TASK-1021）
- **decision_opening-list** — 開局建造清單：起始檔只寫種類與座數，第 2 天照清單蓋到崗位 80%（TASK-1071）
- **decision_opening-works-yard** — 開局補缺口、工地兩倍、公共工程、自動蓋服務建築與營造廠（TASK-1023）

## pitfall
- **pitfall_auto-claim-pending** — 自動建築連挑、服務建築：要扣掉還沒生效的那一份
- **pitfall_auto-cost-swallowed** — 自動建築的造價被同一行的註解吃掉兩天（73b8f68 → TASK-1071 修）
- **pitfall_consume-off-tools** — CONSUME 關掉的小城工具：新需求要照舊
- **pitfall_demand-vs-slots** — 加需求不等於加崗位：名額才是瓶頸
- **pitfall_opening-list-placement** — 幾百座自動建築一次蓋 ⇒ 擺放、重放、規劃過期的坑（TASK-1071）
