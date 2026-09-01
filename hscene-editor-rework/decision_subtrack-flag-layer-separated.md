---
id: decision_subtrack-flag-layer-separated
topic: hscene-editor-rework
title: Subtrack 虛擬優先度層：Flag 層在系統之外(-∞)，不參與競爭只在沒人競爭時接管
type: decision
status: active
created_at: 2026-09-01
created_by: kiara
links: []
related_docs: [Docs/API/UCL_Asset/SkeletonGraphicAsset.md, Docs/API/IAsyncEvents/AsyncSpineAnim.md]
---

Subtrack 是虛擬優先度層，不對應真的 Spine Track：所有層寫進同一條 Track（asset 自己的 Track），
subtrack index 就是優先度，同 index 再播＝整筆覆蓋（剩餘秒數跟著消失）。

⭐ 最重要的拍板：**Flag 層在 subtrack 系統之外**（概念上 -∞）——
不佔格、不進註冊表、任何清除指令碰不到它；有任何 subtrack 在播就無視 Flag、清空就由 Flag 接管。

🩸 我第一版把 Flag 做成「level 0 的一筆註冊」讓它參與競爭，後果兩個：
① AsyncSpineAnim 預設 subtrack=0 ⇒ 事件一播就把 Flag 那筆整筆覆蓋（同層後到者勝）
② ClearAll 時兩個一起消失 ⇒ 畫面空白
而我的第一版修法是「清完重算補回 level 0」—— **那只是把 Flag 塞回競爭場，症狀消失病還在**。

⚠ 仲裁必須是唯一寫入決定點：SkeletonGraphicService.RefreshAnim 是全域的
（任何 Flag 一動就刷所有 asset），Flag 層若直接寫 Track，正在播的 subtrack 會被安靜蓋掉。
⇒ RefreshAnim 只更新 curAnimName，寫不寫得出去由 ApplyOwner 決定。

規格變更（不做遷移）：AsyncSpineAnim 的 m_Track 移除（Track 取自 asset）；
AdvCommandPlaySpine 的 Arg3 語意由真 Track 改成 subtrack。
