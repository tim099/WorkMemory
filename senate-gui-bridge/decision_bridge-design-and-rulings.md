---
id: decision_bridge-design-and-rulings
topic: senate-gui-bridge
title: 三句拍板、協議選型與不降級
type: decision
status: active
created_at: 2026-09-15
created_by: kiara
links: []
related_docs: []
---

**拍板（Tim 2026-09-15，三句）**
1. 文字模式下同時也開視窗（實際等同真實操作）；文字輸出只是方便查看、可以操作、需要時截圖、同時可測 fps
2. **用常駐窗測試（確保是實際流程）**
3. **文字模式主要用來知道視窗上有哪些可互動項目**

⇒ 第 3 句定形狀：**文字不是另一個 renderer，是那顆窗的「這上面有什麼可以點」清單。**

**落地**（Senate `7fd6e42`，6 檔）：`GuiBridge`（協議）／`GuiBridgeHost`（窗端）／`SenateWindow.OnFrameServed`＋注入＋常駐 fps＋`CaptureTo`／`Program.CmdUi` 路由。

**為什麼不沿用 `queues/<lane>/` + `_cmd_results/`**（我把驗收 ① 改寫成實作的樣子，不是偷偷達標）：
那套是 Cmd 派遣的**持久佇列**，`ServerHost` 自己寫著「被切掉的 lane 下次啟動會翻回 pending 續跑」。
⇒ 對一筆扣款是保命；**對一次點擊是災難** —— 窗重開就替你再按一次，而畫面上看不出來。
📌 沿用的是形狀（檔案進出／原子落檔／單一宿主／不發明新機制），不是版面。

**⛔ 不降級**：窗沒在跑 ⇒ exit 3、印怎麼開窗（照 Tim 2026-09-02 ⑦ 對 Server 的既有拍板）。
`--local` 是顯式退路，第一行必定自報「這不是窗上的畫面」。

**閒置成本**：`OnFrameServed?.Invoke` ＋ `if (!m_Signal) return;`。Watcher 推、心跳在自己的 thread。
對拍（同 build 組態、n=3 對 n=3，另建 worktree 拉 HEAD 當基準）：59.4/60.1/60.1 vs 60.1/60.0/60.1。
⚠ 射程：**60 fps 是 vsync 天花板** ⇒ 量得到「有沒有凍住」，量不到「有沒有慢 3%」。
