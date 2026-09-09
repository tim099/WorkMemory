---
id: pitfall_measuring-mainthread-stall
topic: unitask-editor-async
title: 量「主緒被誰佔住」的三個地雷：探針覆蓋率、量具與被測者同緒、拿掉粗鎖會提高既有競態發生率
type: pitfall
status: active
created_at: 2026-09-09
created_by: summit
links: []
related_docs: []
---

**兩格關於「量主緒被誰佔住」的踩坑，都是 2026-09-08~09 現場量的。**

## ① 「零命中」的成因可能是探針位置，而它跟「不成立」同形

我在 `UCL_ChatTavernIO_PerMsgFile` 的三個**讀路徑**（`LoadAllMessages`/`Tail`/`LoadMessagesAfterSeq`）埋等鎖探針，
零命中 ⇒ 我推「鎖是無辜的，主緒是被別的東西佔住」（甲乙二分裡選了乙）。

**錯的不是那個零，是我對零的讀法。** 真兇是 `GetSortedMessageFiles/read` ——
一個**只想讀一個 Dictionary** 的鎖點，被抱著鎖做全量檔案讀的那條路拖了 2290ms。
它在補探針那天之前**完全不會出聲**。

⇒ 判準：**量具的覆蓋率不足不會自己出聲**，而「我插的點沒叫」與「沒有人在等鎖」在輸出上一樣。
⇒ 動作：宣告「不是 X」之前，先問「我的探針蓋到 X 的全部入口了嗎」——答不出來就只能說「我插的那幾個點沒叫」。

## ② 站在主緒上的量具，被測者停了它也停

`kind=stall`（每幀戳 UtcNow）與 lock-wait 探針**都在主緒**⇒ 主緒凍住時它們一起凍
⇒ 它們只能在**事後**補記，答不出「凍的當下發生什麼」。
補法是一條獨立背景緒的 watchdog（`UCL_AgentCmdMainThreadWatchdog`）：每 250ms 讀主緒心跳，
凍 ≥3000ms 就落 `kind=freeze`，並在**凍結進行中**抓鎖持有者快照 ⇒ 那一格才分出了甲乙。

⚠ 三個實作地雷（都是踩過才寫的）：
- 心跳要用 `Interlocked` 另存 ticks —— `DateTime` 是 8 bytes struct，跨緒讀寫會撕裂 ⇒ 假的 freeze 行。
- watchdog 必須在 `beforeAssemblyReload` 收掉 —— 背景緒**不隨 domain reload 死掉**，
  漏收的樣子是每次編譯多留一條，寫出重複行**看起來像「凍了很多次」**（假讀數比沒有量具貴）。
- 鎖持有者快照要用 `using var` 作用域（含拋例外自動清）—— 殘留的 holder 會長成「有人抱鎖 40 分鐘」的假讀數。

## ③ 一格判準（Tim 2026-09-08 拍板的方向）

臨界區只包**寫入段**，鎖內不做 IO。三個讀路徑的粗鎖拿掉、快取換 `ConcurrentDictionary`，
冪等性論據是「快取值是不可變的解析結果 ⇒ 兩條緒各寫一次同一鍵是冪等的，最壞多解析一次同一個檔」。
⚠ 而拿掉粗鎖會**提高**既有競態的發生率（`m.seq` 寫回共用快取實體那隻）——
那不是新 bug，但發生率是我改出來的 ⇒ 同一輪要一起修，不然它會變成「別人的 bug」。
