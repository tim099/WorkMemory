---
id: pitfall_models-hp-powder-traps
topic: constitution-broadside
title: 長砲改模型、實際血量、殉爆：踩到的坑
type: pitfall
status: active
created_at: 2026-10-12
created_by: ame
links: []
related_docs: []
---

長砲改成模型（2026-10-12 晚，ArtGallery 工作區）踩到的坑：

1. **外框判命中太寬**：長砲外框 1.3 × 3.1 m，空的角落也算 ⇒ 砲甲板一半的彈都打到砲（40 秒掉 26 門）。改成網格轉佔用格（`meshOcc`：三角形每 0.4 格取一點、往法線反方向半格算實心、外擴一格；`inMesh`）⇒ 15 門。鏈彈仍用外框＋1 m（它本來就橫掃 2.4 m）。
2. **平面的「佔住」**：makeCrew 以前用 (x, y) 標模型外框 —— 長砲變模型之後把上一層 spar deck 的站位佔掉，艙底火藥桶更佔掉大半平面（少 16 個砲手）；舵手的站位一直在舵輪外框裡（從沒站上去過）。改成照高度判（`blocked`），舵手站舵輪後面。
3. **彈藥庫打不到**：砲彈碰海面就停（`b.p[1] < 0`），彈藥庫頂在水線下 2.3 m。火藥桶血量拉到最低 20、三種子 90 秒：一桶都沒碰到。要讓它在對戰裡發生得另給一條路（等 Tim）。量測用 `__game.ignite(艘, 0|1, 'keg'|'mag')`。
4. **大爆炸要走區塊**：shatter 的三重迴圈對 9.5 m 球要走 2,800 萬格；`crater` 只走有東西的 16³ 區塊（寫入會換掉 g.tab 那一格 ⇒ 照原本那份走）。斷船不能靠一般連通判定：龍骨全段都是錨 ⇒ 兩半都連著；斷船後 `sh.keel` 只留長的那半截。
5. **進水沒有上限**：殉爆打掉三十幾萬格、兩秒就沉完 ⇒ 進水上升速度上限 0.25 m/s（平常戰鬥碰不到這個值）。
6. **修改腳本**：js 檔是 CRLF；Python 以文字模式寫會在 Windows 變 CRLF、bash heredoc 裡的 `\r\n` 會被吃掉。用 scratchpad 的 patchlib.py（讀進來轉 LF、比對、存回 CRLF）；sed -i 會把整檔改成 LF。
