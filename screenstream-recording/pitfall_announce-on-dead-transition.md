---
id: pitfall_announce-on-dead-transition
topic: screenstream-recording
title: 開播廣播掛在一個再也不會發生的 transition 上（靜默消失 3 天沒人發現）
type: pitfall
status: active
created_at: 2026-08-04
created_by: unknown
links: []
related_docs: []
---

**症狀**：Discord 收不到 ScreenStream 開播廣播。

**根因**：廣播原本掛在 daemon 端「cfg.enabled false→true」的 transition。而 **2026-07-28 daemon 生命週期改成「存活綁 enabled」** —— 開播時頁面先寫 enabled=true 再 spawn daemon，daemon 啟動時 last_enabled 已是 true → **那個 transition 再也不會發生**；停播時 daemon 直接被 kill → 也沒機會發。start/stop 兩個廣播一起靜默消失。

**證據鏈**（值得學這個查法）：
1. 酒館最後一筆廣播 2026-07-27，而生命週期改動是 07-28 —— **日期完全對上**
2. daemon log（08-01→08-04, 157 行）內 announce 這個字出現 **0 次**，而那支函式**成功與失敗都會 log** → 不是發失敗，是根本沒被呼叫
3. Discord 那端無罪：IsRealAgentSender 擋 tavern-keeper 是**經濟結算**用的（防 token_parse 把酒保訊息當指令），不管 mirror；Tim 手機收得到打款通知就是 keeper 發的

**修法（B 案，Tim 拍板）**：廣播搬回 Editor 按鈕端（UCL_ScreenStreamPage.PostStreamAnnounce）—— **事件的所有者是那顆按鈕，不是 daemon 的存活狀態**。daemon 端 post_bartender_announce **整支刪除**（60 行）：留著就是同一段文案兩處各存一份（必漂），而且生命週期若改回常駐 idle 會雙發。

**通則**：拆掉一個機制時，掛在它上面的東西不會舉手。改生命週期的人正確處理了 _live_info 對齊（還特別註解「daemon 重啟不觸發 transition」），但廣播搭在同一個 transition 上，跟著一起被切斷。
