---
id: pitfall_long-river-harbour
topic: blocksail-europe-ports
title: 長河的港：harbour_deep 要帶 --path
type: pitfall
status: active
created_at: 2026-11-02
created_by: basecamp
links: []
related_docs: [docs/TOWN_TOOLS_TERRAIN.md, docs/WORLD_FORMAT.md]
---

長河的港（倫敦、塞維亞、阿姆斯特丹）的水道：harbour_deep 要帶 --path（TOWN_TOOLS_TERRAIN §3「長河的港」）。
- 不帶的話：水道抹圓描邊後只留最大那一塊 ⇒ 窄河斷開，範圍只剩河口，港口照舊 0 m（2026-11-02 試倫敦 68 km：範圍只有 0.39～0.74°E）。
- 河只有一兩百公尺寬（塞維亞阿雷納爾前 150 m）⇒ 另要 --shore 10 --ramp 15,40,80，不然預設的 30＋40 m 一退一縮只剩一條縫（港口 −0.17 m ⇒ 改後 −5.0 m）。
- harbour_grid 要取到河口外：倫敦 r 64 km／96 m、塞維亞 r 85 km／64 m、阿姆斯特丹 r 100 km／128 m。
- 🩸 別再猜「底稿沒有圈」：陸地本來就在每一層「比 D 淺」的圈裡；先量範圍有沒有蓋到港口。
- --apply 會讓 carib 10 檔＋ean40-20、ean48-14 漂（工具本身的漂移）⇒ 照 HEAD，index.js 只接新的筆（記憶 blocksail-bathy-base-stale）。
