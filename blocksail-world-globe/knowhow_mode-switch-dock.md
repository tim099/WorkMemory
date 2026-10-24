---
id: knowhow_mode-switch-dock
topic: blocksail-world-globe
title: 遊戲裡換玩法、停靠出航（TASK-0867）的拍板、坑與量法
type: knowhow
status: active
created_at: 2026-10-24
created_by: unknown
links: []
related_docs: []
---

遊戲裡換玩法（TASK-0867，Blockside src/js/88_modeswitch.js；Blocksail globe.html docksFor／mode）——拍板、量法與坑：

拍板（Tim 2026-10-23～24）：
- 停靠：按 🏘 就當場換城鎮模式、建築模式直接開；船「物理照跑（會晃）、強力拉著沿航線停進去」（dockPull：航線上的點照航速走，位置船頭每秒 ×3 彈簧拉）。只考慮航速。
- 停靠鈕：城鎮範圍（照重心放大兩倍）或那一處的進港航道上（兩側 60 m），且那一處 400 m 內擺完；同時兩座城 ⇒ 照離停靠處的距離。圖示 🏘（跟拋錨 ⚓ 分開）。
- 出航：物理開（帆與俥鐘交給自動開、倒車中也接得過來），沿航道開到外端才交還。

坑：
- 以前「開到 ⇒ 下錨 ⇒ 繞錨轉到船頭 ⇒ 才換城鎮」：風、水流推著轉不到 ⇒ 永遠停在航海、沒有 🔨（Tim 的實機；我的測試剛好都轉得到）。⇒ 換玩法別綁在物理收斂上。
- 君士坦丁堡、加拉太的範圍貼著海岸畫，金角灣不在範圍裡 ⇒ 照字面的「在範圍裡」永遠沒有鈕。
- 大城地面區照收塊一直重建 ⇒「附近擺完」時有時無 ⇒ 擺完過一次就記著（DOCK.okd）。
- 卡住判準要用模擬時間想：自動開時模擬 ×4，25 秒 ＝ 現實 6 秒。
- 換進城鎮要關跟船（camSet track false），不然船開進來那段把鏡頭帶到海上。

量法：scratchpad tpre/mode_test.cjs（有視窗 Chrome，地球儀開航海；NOW=1 按停靠就換那一版、BACK=1 停好回地球儀、START=out 從航道外端、REV=1 倒車中按、FLIP=1 剩 45 m 把船頭轉 180°、ANCHOR=1 等大城載完先下錨、視窗擺在 x −2400 不擋 Tim）。
