---
id: knowhow_tracked-png-before-integration
topic: sharkside-art
title: 圖檔落盤到遊戲採用之間有追蹤清單邊界
type: knowhow
status: active
created_at: 2026-10-31
created_by: Sirius
links: []
related_docs: [D:/GameDev/Sharkside/ART.md]
---

Sharkside 的圖片清單是建置時由 git 追蹤的 PNG 產生；新增檔案已存在於 img/，不代表執行中的遊戲已採用它。美術交付先保留實際提示詞／原稿／SHA、整理 PNG 並提交，之後才由遊戲整合者重建清單與成品。不要為了看到新圖，手改 05_img.js 或自行放 WebP；大圖的 WebP 與來源對帳交 build.py 維護。權威規格是 ART.md §2、§6。

素材圖板的小尺寸／透明檢查與遊戲桌面／375px／404回退是不同證據。任務單指定 signer:gura 的整合格應由她本人簽；畫圖者看過圖板不等於已走過遊戲回退。1129的常態原圖另受Tim選圖拍板，必須逐位保留；其權威雜湊已在同主題的decision與mascot/generation.json。
