---
id: pitfall_ui-hud-relayout-traps
topic: constitution-broadside
title: 介面重排一整串的坑（無頭最窄 500、rAF 背景停、innerHTML 重畫吃 click、帆雙層抵銷、?cmd 讀太早、固定風偏一邊、拿掉文字前先想用途）
type: pitfall
status: active
created_at: 2026-10-14
created_by: ame
links: []
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-14 介面一整串（TASK-0552／0564／0566／0567、隨機風、資料夾搬家）踩到的：
- 量具：無頭 Chrome `--window-size` 寬度最小 500（375 會被撐成 500 再截掉右邊）⇒ 手機版面一律在預覽窗格 resize_window mobile 看；全解析度截圖用無頭 1920×911。
- 預覽窗格在背景時 rAF 停 ⇒ HUD 不畫、find 抓不到按鈕；先 screenshot 一次讓它動起來。zoom 的 region 座標系常對不上，改 screenshot＋scale。
- 測 UI 用 `_uitest.html`（成品尾巴加一段：等 __vxk.demo、照 ?open= 點一顆）放在成品旁邊，截完刪；檔名開頭底線，別提交。
- 卡片每 6 格整個 innerHTML 重畫 ⇒ click 要按下放開同一個元素，跨過重畫就不觸發 ⇒ 用 pointerdown（Tim 抓到「收起有時沒反應」）。
- 帆的三角形是正反兩層、繞向相反 ⇒ canvas 一條路徑全部 fill 會互相抵銷成空的 ⇒ 一個三角形填一次。
- `?cmd=` 在 52_fleet.js 讀時 53_commander.js 的 CMD_NAMES 還沒定義 ⇒ 整頁停在載入；網址參數依賴後面檔的常數 ⇒ 放到 69_prefs.js 讀。
- 風原本固定往 +z ⇒ 紅方永遠上風（12 場紅贏 10）；風隨機後邊的偏差消失（36 場 −0.07）。對拍沙盒一定要兩邊互換配對。
- 每次只改畫面也要反向對照：`git show HEAD:<成品>` 到 _headtest.html、同一組 ?demo=60&dump=1 cmp、?demo=75&still=1&cam=deck 縮圖 hash（現在 f05b7771）。同時跑太多無頭 Chrome 會有幾個沒倒出 #dump（資源不夠）⇒ 對拍那幾個一個一個跑。
- 拿掉常駐文字前先想它還有沒有別的用途（「我艦→敵艦」其實是鏡頭名稱，拿掉後 Tim 說看不出鏡頭 ⇒ 補回）。
- 建置走 `python -I admin.py build`（gura 的；設定檔內嵌）；成品在 HtmlGames/ame_constitution_broadside/（HtmlGames 這一層只放 .md）。
