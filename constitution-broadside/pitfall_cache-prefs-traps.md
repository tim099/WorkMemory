---
id: pitfall_cache-prefs-traps
topic: constitution-broadside
title: 設定存檔與船型快取的坑（_cache 換船、只存非預設、壞存檔別順手蓋、快取鍵、驗證法）
type: pitfall
status: active
created_at: 2026-10-13
created_by: ame
links: []
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

TASK-0520（設定存檔＋船型快取，2026-10-13）踩到的：

- **_cache 不進版控 ⇒ 換台機器建置會換船**：Florin 這台的 VoxelKit/_cache/constitution.vxk 是 10-12 14:30 的舊版（600 KB、沒有長砲模型），HEAD 成品內嵌的是 BTC 那台匯出的 1.1 MB。直接建置 ⇒ 戰局整個不同、縮圖 hash 6002e5c7 ≠ 51b2d06b，看起來像改程式改壞了。
  查法：比兩份成品內嵌 #ship 的 sha256；修法：從版控成品的 #ship（base64）解回 _cache。只改程式時一定先比。
- **只存跟預設不一樣的**：全部存 ⇒ 玩家沒動過的預設值被凍在存檔裡，之後改預設（火藥 450 → 675 那種）老玩家吃不到。
- **打開壞存檔不能順手存預設**：PREFS.last 若設成原字串，關頁的 prefsSave 會發現不同 ⇒ 把看不懂的那筆刪掉。基準要用「套用後的 prefsSnap」，玩家動了才寫。
- **自動開砲的存法**：換了模式還沒開局時，AUTOFIRE 還沒照新模式重套（reset 才套）⇒ 這時不存 auto（LAST_MODE !== MODE 就跳過）；存了 auto 的話載入時 LAST_MODE = MODE，開局不會被重套蓋掉。
- **快取鍵用原文雜湊最省事**：內嵌 .vxk 的 base64（約 0.8–1.5 MB）FNV 一次約 2 ms，繩帆模型改了也抓得到（header 的 checksum 只管格子）；程式版本 ＝ document.scripts 裡內嵌 JS 原文的雜湊，改任何一行都清，不用手動升版號。
- **IndexedDB 直接存結構化物件**：布模板（物件＋typed array）不用自己序列化；同一次 put 裡共用的參考會保留。量過：船體網格 CPU 240–500 ms，讀回 6.8 MB 約 20–60 ms。put 的結構化複製在主執行緒（約 0.2 秒）⇒ 載好之後才存。
- **驗證工具**：__game.hullCheck() 逐位元組比讀回的與現場算的；模型網格的頂點從 GPU 讀回（gl.getBufferSubData）。sandbox iframe 的讀數看無頭 Chrome 截選單那一格（載入那一行會寫「存不了」）。
- 設定檔驗縮圖：無頭 Chrome 帶固定 --user-data-dir，先開一頁臨時頁（畫廊根目錄，驗完刪）寫存檔再轉到遊戲建快取，再用同一個設定檔截 ?demo。來源要一致（都用 127.0.0.1）。
