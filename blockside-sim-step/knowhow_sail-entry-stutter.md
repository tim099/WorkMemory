---
id: knowhow_sail-entry-stutter
topic: blockside-sim-step
title: 進航海卡頓怎麼量、量到什麼（0816／0817／0818）
type: knowhow
status: active
created_at: 2026-10-22
created_by: erina
links: []
related_docs: []
---

進航海卡頓（TASK-0816／0817／0818，2026-10-22）怎麼量、量到什麼 —— 下一個人別重猜：
- 量法：有畫面的 Chrome＋CDP（Profiler＋iframe 的 rAF）；A/B 不開 Blocksail worktree，起一個小伺服器只把 /battle.html 換成要測的那份（admin.py shell 指 core）。iframe 的 contentWindow 換頁後是同一個物件 ⇒ rAF 照 document 掛。
- 靠岸的點才看得到（皇家港外 -76.835,17.925；君士坦丁堡南 28.965,40.997）；開闊海面那點把 10 fps 的情況藏起來了。
- 兩次第一猜都錯：「遠景載入」其實是低於海平面的遮罩整窗重算（已搬 Worker：bedDryCalc）＋著色器同步編譯（地形 P.bed 一支 2.6 s，改 glWarm／glProgTry 背景編）；「大城一口氣載入」其實建築很便宜（12,854 個 44 ms），貴的是小地圖每格重畫、路面距離場、地面整座重建。
- 還超過 8 ms 背景預算的：城鎮整地 townHv（38～56 ms）、地面一區 17～22 ms。
- 岸邊閃不是遮罩漏遮，是浪爬上 0 m 上下的沙灘 ⇒ P.water 照水深縮浪（SEA_SHORE）；水邊不再跟浪跑。
