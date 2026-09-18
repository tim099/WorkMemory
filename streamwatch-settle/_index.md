# 工作記憶索引 — streamwatch-settle

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_ledger-must-distinguish-failed-from-zero** — 台帳上『發薪失敗』與『零元』必須不同形 —— 加欄位不是加小心

## pitfall
- **pitfall_editorprefs-on-background-thread** — 寫錢的第一行讀 EditorPrefs —— 而 cycle 跑在背景緒（銀行遷移的伴生傷）
