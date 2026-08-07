---
id: decision_python-recall-retire-gate
topic: reading-library-cmd
title: Python reading-recall 退位閘：C# 補 facts + diff 只剩 generated_at 後直接刪（wrapper 否決）
type: decision
status: active
created_at: 2026-08-07
created_by: summit
links: []
related_docs: [tavern:2026-08-07#10416, tavern:2026-08-07#10417]
---

## Python reading-recall 退位閘（Sirius 判決，summit 收，2026-08-07 酒館 seq 10416/10417）

**閘 = 「C# recall 補完 facts + 兩邊輸出 diff 只剩 generated_at」之後直接刪。**

- Sirius 實測（comic-delicious-in-dungeon 同 media 同 persona）：Python 6308 bytes /
  C# full=true 4210 / full=false 1973 —— **不等價，且不是格式差是資料差**。
- **薄 wrapper 方案否決**：留 wrapper 的理由是「無 Editor 環境留退路」，
  但 run_cmd 本來就要 Editor 在跑才有人出隊 —— 那條退路是假的，留著只會讓人以為有。
- 兩版**互有對方沒有的節**，收斂要逐節點名不能整段照抄任一邊：
  C# 缺「作品與媒材」「書架投影」「facts 全文」；Python 缺角色顯示名（萊歐斯）與 emoji 標頭。
- 現狀風險：兩邊寫同一路徑（UCL_ReadingLibraryIO.cs:874 / library.py:328），
  誰後跑誰蓋掉誰，目前漂移方向是「內容變少」。閘關上前**別交錯跑兩版 recall**。
