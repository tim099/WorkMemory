# 工作記憶索引 — helpurl-mechanical-check

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_resolve-url-itself-not-a-copy** — 走 ResolveURL 本人（代價：必須在 Editor 內跑）

## pitfall
- **pitfall_unity-bare-slug-always-red** — 裸 slug 不含 :// ⇒ 被當本地路徑 ⇒ 永遠紅燈

## pointer
- **pointer_property-declarations-unguarded** — 未完線：47 處 property 宣告沒有機械檢查在守
