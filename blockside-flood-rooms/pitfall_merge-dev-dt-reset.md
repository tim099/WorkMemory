---
id: pitfall_merge-dev-dt-reset
topic: blockside-flood-rooms
title: 合併 dev：開局重設步長、讀數跟著 dev 走、?demo 對拍被分頁影響、Blocksail 要重套 battle.html
type: pitfall
status: active
created_at: 2026-10-22
created_by: calli
links: []
related_docs: [D:/Unity/Blocksail/external/Blockside/src/js/81_floodlab.js, D:/Unity/Blocksail/external/Blockside/src/js/70_flow.js]
---

合併 dev 時的兩個坑（2026-10-22，calli/flood-research → dev 84b5b45）：
- 開局會重設步長：TASK-0805 之後 reset()（70_flow.js resetSteps）照物理設定重設 DT、DT_BASE。報表要換步長（81_floodlab.js 的 hz=）⇒ ⛔ 在 reset() 之後換，DT 與 DT_BASE 一起換、量完一起還原。
  在 reset() 之前換的話，合併文字上沒有衝突，報表照樣出得來，但 hz=30／10 會被蓋回 60。驗法：同一個情境 hz=30 與 hz=10 的讀數要不一樣。
- 步長的讀數會跟著 dev 走：TASK-0798 讓水格的節拍照 60 Hz 換算（33_flood.js floodStep 的 steps60(L.ev)）之後，TASK-0806 的情境重量：
  30 Hz 從 −61% 變成 −38.9%（142 s 時 433.8 m³，跟 60 Hz 幾乎一樣），10 Hz 從 −82% 變成 −71%（206.2 m³；900 s 時 779 m³、back 402）。0808 要對比，用合併之後的數字。
- ?demo 對拍會被分頁影響：a40e08b 照鏡頭讓船員降級（03_camera.js camShipSee：鏡頭還沒算過才不降級）。
  在 app 的瀏覽器裡，剛開的分頁第一次載入會跑出別的值（demo 420 blast 10：dev 自己 def54446、合併版 de797bc4，畫過畫面的分頁兩邊都是 ae7c9058）。
  ⇒ 兩邊都用已經畫過畫面的分頁，或用無頭；一次對不上先換分頁再跑一次，不要直接懷疑自己的改動。
- Blocksail 同步指標時要一起重套 battle.html（python -I external/Blockside/src/admin.py shell battle/shell.json），提交前的鉤子 tools/battle_hook.py 會檢查。
