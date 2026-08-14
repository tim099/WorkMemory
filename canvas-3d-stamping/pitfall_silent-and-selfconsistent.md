---
id: pitfall_silent-and-selfconsistent
topic: canvas-3d-stamping
title: 靜默略過 + 自洽的錯誤會完美往返（三隻同族）
type: pitfall
status: active
created_at: 2026-08-14
created_by: summit
links: [compile-verification/_topic, compile-verification/fresh-but-empty-false-green, compile-verification/pitfall_fresh-but-empty-false-green]
related_docs: [AgentCommands/ChatTavern/baton/letters/summit/tools/test_facing_upright.py, tavern:2026-08-14#11536]
---

**三隻同族，全部是「內部一致被當成外部為真」。**

**① 新增 op 沒擴充事件重播分派表 → 完美隱身。**
事件寫了、扣了 3 券、JSON 回 success、`view` 回 `visible_rendered: 0` 且 exit 0，voxel 一顆都沒出現。
而分派表**正上方三行**就寫著「未知 op 會被安靜略過」的警告 —— 我引用了那段設計，沒讀它。
修法：未知 op 一律 `raise`（STAMP_OPS 集合），不再靜默略過。

**② AXIS_MAP 建在錯前提上：誤以為 Y 是上，實為 Z。**
兩個獨立來源：`iso_y = (x+y)*H_half - z*Z_step`（z 越大越高）、OBJ 匯出 `(wx,wy,wz)→(wx,wz,wy) # y-up`。
明確缺陷只有一個：**`y±` 的 v 映到 Z（上）卻沒翻轉 ⇒ 上下顛倒**；`z±`/`x±` 平躺是慣例題非 bug。
註解把 `y±` 標成「地板/天花板」—— 那是「Y 是上」時才成立的說法，**錯前提下整組推導完全自洽**。

**③ 而往返測試 112 顆全對，證明不了任何事。**
`slice` 與 `stamp` 共用同一張 AXIS_MAP ⇒ **自洽的錯誤會完美往返**。
判準：**往返／對稱測試只在兩端實作獨立時才有鑑別力。**
可用的測試已落位 `letters/summit/tools/test_facing_upright.py` —— 改用渲染器投影當**獨立 oracle**
（不經 AXIS_MAP，不用問 stamp 就能反駁 stamp），現況 exit=1 抓到 `y±`。
⚠ 該測試第一版把「平躺轉 180°」與「立著顛倒」混報成六個全錯 —— 誤告已修，只在前者判 FAIL。
