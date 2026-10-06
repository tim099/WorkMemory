---
id: pitfall_gui-page-per-frame-io-and-cli-driving
topic: senate-gui-page-catalog
title: 頁面每幀碰磁碟會卡死、CLI 驅動一次一個動作
type: pitfall
status: active
created_at: 2026-10-06
created_by: unknown
links: []
related_docs: []
---

**症狀**：頁面在 ImGui 視窗裡嚴重卡頓（persona 管理頁 0.1 fps，一幀 7.6 秒）；CLI 驅動 `ui --set … --click …` 回成功卻寫到別人身上或寫空值。

**可行動守則**
1. 繪製函式不碰磁碟：視窗每幀重畫。讀取在 OnPush／「重新讀取」做成快照，換人只算那一位，寫入成功只重讀那一位（GetRaw 每位 0.1–0.3 秒，隨 wakes/ 信數成長）。頁首註明「畫面是快照」。
2. 清單不畫縮圖：每張圖第一次畫要解碼（12 張 10 MB ≈ 1.5 秒），只在選中那位才載。
3. 欄位倉對齊不要「每次進頁就對齊」：CLI 每道指令都是新的進頁，上一道 --set 會被蓋回磁碟值。改記「對齊的是哪一位」（存在欄位倉裡），換人／重新讀取／存檔成功才對齊。
4. CLI 一次只做一個動作（現在多給就 exit 2）；下拉＝`--click <id>` 展開（切換！先 --list 看現況）→ `--set <id>/search=` → `--click <id>/pick/<值>`；`--set` 指到按鈕會被擋。
5. 量幀率：`senate ui --soak N --page <key>`；「其餘最慢」通常就是開頁那一下被算兩次，對照別頁再下結論。

**出處**：TASK-0424（2026-10-06，erina），commits 62d0882／d6ffa1a／f5706f1、SCP_Core 4e78fb2。
