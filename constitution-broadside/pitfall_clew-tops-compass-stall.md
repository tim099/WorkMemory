---
id: pitfall_clew-tops-compass-stall
topic: constitution-broadside
title: 帆腳、桅頂、手機羅盤、頂風失速的坑（canvas 預設 300×150、帆腳跟 sheet 那根、離線模型調參）
type: pitfall
status: active
created_at: 2026-10-13
created_by: ame
links: []
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-13 上午（帆腳、桅頂、手機羅盤、頂風失速）踩到的：

- **canvas 預設 300×150**：羅盤只在「寬 ≠ 150×dpr」時設尺寸 ⇒ dpr 2（手機）寬剛好 300、高停在 150 ⇒ 拉成兩倍高、下半截切掉。電腦 dpr 1／1.25 看不出來。⛔ 設 canvas 尺寸一律寬高都比。手機版面用瀏覽器 resize_window mobile（375×812、dpr 2）重現。
- **帆腳跟哪根帆桁**：跟「它的 sheet 綁的那根」（clewYard），不是自己那根。course 綁甲板繩栓 ⇒ 不轉（basecamp）；topsail 以上 ⇒ 下一根（各層帆桁在作品裡的扭角不同，跟錯差 0.3–0.5 m）。打帆的判定照高度在帆頂與帆腳轉角之間內插（footYard）；spanker 沒帆腳 ⇒ boom。
- **桅頂找法**：從 z 200 往上找第一層「站得住的點 ≥ 6」的平台；⛔ 不夠四個就往上找 ⇒ 爬到高處橫桁（377–465）。改了站位人數 ⇒ 亂數走法變、縮圖整個換。
- **頂風失速的調法**：遊戲裡一組情境要跑 1–2 分鐘（javascript_tool 45 秒逾時 ⇒ 背景跑、MessageChannel 讓出、結果存 window 再讀）。先寫離線的簡化模型（β、航速、舵、推力）掃參數，跟遊戲裡第一組讀數對上（37°／25°）再信它，最後回遊戲實測。
  第一版（推力 0.03、失速時減速 8 秒）寫實連帶 6.5 節都搶不過；定案推力 0.02、減速不另外加快、航速一半以下才推得動。
- **電腦操舵不吃失速**：它不會搶風，吃了會卡死 ⇒ 失速只管手動（sh.manual）。
- **縮圖會換的改動**：帆的形狀（物理在快轉最後 6 秒開）、站位人數、航速倍率都會換縮圖；換了就重截 RawImages 的縮圖、對一下展品卡的說明還對不對。
