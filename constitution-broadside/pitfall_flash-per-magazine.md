---
id: pitfall_flash-per-magazine
topic: constitution-broadside
title: 火光竄下艙口照取用點算 ⇒ 一次爆炸算兩次（娛樂火藥的讀數都偏）；先後要分清楚
type: pitfall
status: active
created_at: 2026-10-13
created_by: ame
links: [constitution-broadside/pitfall_models-hp-powder-traps]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-13（餘燼號中央彈藥庫、娛樂火藥 300 → 450 之後）：

- **火光竄下艙口以前照取用點算冷卻**：下層、上層砲甲板的取用點上下疊著（相距 2.3 m ＜ FLASH_R 3 m），一次爆炸竄下兩次 ⇒ 門邊的桶一次吃 600。
  「要竄下去幾次才會著」的門檻（675 ⇒ 3 次、450 ⇒ 2 次、300 ⇒ 1 次）整個被打穿。改成同一間彈藥庫 3 秒一次（mag.flashT）。
  ⛔ 之前所有「娛樂火藥多少剛好」的讀數（300、450 的第一輪）都是在這個 bug 底下量的，Tim 調數字的依據跟著偏。
- **怎麼抓到的**：讀數只說「第 5 秒殉爆、藥包炸 47 次」，看起來像送藥手擠成一團連鎖。一格一格（1/60 秒）印彈藥庫的桶（lit／dead／hp）與每個取用點的 flashT：
  桶先全滅、取用點的桶一個都沒少 ⇒ 不是連鎖；兩個取用點的 flashT 同一刻 ⇒ 兩次火光。藥包的 47 次是殉爆之後的結果，不是原因。
  ⇒ 數字的先後要分清楚：只看結束時的累計，原因和結果會混在一起。
- 取用點分到前後（只有一間彈藥庫時）有用但不是主因：6 個種子 2 → 1 場；修火光之後 0／6。
- 量測：無頭 Chrome 平行 `?demo=180&dump=1&seed=&ship=a,b&funpowder=`（dump 有 dead／cart／magGone）。15 個同時跑會有 3 個沒寫出讀數（資源不夠）⇒ 12 個以內。
