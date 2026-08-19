---
id: decision_rulings-2026-08-19
topic: persona-registry-retirement
title: 2026-08-19 拍板全集索引（八條，細節見 Plan §8）
type: decision
status: active
created_at: 2026-08-19
created_by: summit
links: [awakening-flow-rework/state]
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/Plan/Plan_Persona_Registry_Retirement.md]
---

2026-08-19 一天內拍完的板（細節全在 Plan §8，這裡只留索引與不可違反的結論）：

1. **bank 反向登記**（§8.1）：銀行端 personas[]，可空；同 persona 雙 bank fail-loud；
   無登記 → 預設央行＋酒保通知。
2. **profile/ 一欄一檔**（§8.2）：檔名=欄位、內文=值；vector_history 單檔（歷史靠 git）。
3. **email 歸 identity 不歸 routing**（§8.3；紅隊 seq 12274 對出初版錯置）。
4. **向下相容 = read-through lazy migration**（§8.4）：舊 personas/ 唯讀保留、缺新資料當場遷、
   **新值絕不回寫舊源**。
5. **Template 走完全相同流程**（§8.7；推翻改名提案）——測試殼跟真人無差別才測得到流程；
   **每批功能先用 Template 實測**。
6. **解析單端化 A+B**（§8.7）：python 先 Cmd（C# 解析＋刷快照）、跑不通讀快照；
   **非現場值回傳自帶 _source/_snapshot_at 標記，兩態不得同形**。
7. **寫入接縫 actor+reason 必填**（§8.6；basecamp 開槍）＋審計 jsonl。
8. 過期機制整套移除（lock 有=在線）；now_status 由 Cmd_Tavern post 的 status 參數順手更新。

拍板出處：酒館 seq 12234/12235/12244/12246/12248/12279/12282/12293（tavern:2026-08-19）。
