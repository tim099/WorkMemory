---
id: pitfall_pinned-scope-and-probe-subject
topic: senate-gui-bridge
title: 釘住(Pinned)的作用域，與探針打錯容器（TASK-0236）
type: pitfall
status: active
created_at: 2026-09-17
created_by: gura
links: []
related_docs: [task:TASK-0236, commit:36cdccf, commit:f47f1fd, src/Senate.Desktop/GuiImGuiRenderer.cs, src/Senate.Desktop/SenateWindow.cs]
---

## 症狀與根因（TASK-0236，Tim 實測推翻三次）

`SCP_GuiToolPage` 的 TopBar 跟著內容捲走。根因兩層疊在一起：

1. `GuiImGuiRenderer.Render` 舊版只掃 **`iRoot.Children`**（直接子節點）找 `Pinned`
2. 而 `SCP_GuiPageController.Draw` 用 `iUi.IdScope(page.Key)` 包住整頁，`IdScope` **會 Push 一個 Column**

⇒ TopBar 掉在 Root 的**孫層** ⇒ 一個都找不到 ⇒ 整棵樹（含 TopBar）被丟進會捲的子區域。
⛔ **沒有任何一層會喊** —— 釘住失效之後，畫面看起來只是「這一頁會捲」。

## 修法選型：改 renderer，⛔ 不改 IdScope

讓 `IdScope` 不建節點是**更小的 diff**，而我沒選它：
那只把樹「這一次」擺回正確形狀，任何人以後在頁面外多包一層群組就**靜默**重現同一隻 bug。
⇒ 釘住與否是 renderer 的概念，**找得到它是 renderer 的責任**。

落地（`Senate 36cdccf`）：
- `CollectPinned` 前序走整棵樹（⛔ 不往釘住的節點裡面再找 —— 它的子孫是它自己的內容）
- 守衛放在 `RenderNode` **開頭**而不是各容器的迴圈裡：Row／Column／Box 都經過那裡
  ⇒ 不管掉在第幾層都跳得掉；分散在各容器的話，漏掉的那一種**不會報錯**，
  只會讓頂欄在畫面上出現兩次（而那看起來像版面壞了，不像釘住壞了）

## 🩸 而真正讓它誤判三次的不是那隻 bug，是**探針打錯容器**

舊探針對**外層視窗**呼叫 `SetScrollY`，而外層帶 `NoScrollbar | NoScrollWithMouse`
⇒ `ScrollMaxY == 0` ⇒ 被夾回 0、畫面完全不動
⇒ 它每次都印 `ScrollY=0 / ScrollMaxY=0`，而那**跟「釘住了」長得一模一樣**。

⇒ 探針移進 renderer（`ContentScrollProbePx`）—— 只有那一層知道會捲的子區域存不存在、叫什麼。
⇒ 讀數**強制指名容器**：`scp/content: ScrollY=… / ScrollMaxY=…`。
不指名的讀數沒辦法分辨「這一格捲不動」與「我量錯了另一格」。
⇒ 外層視窗的讀數**與子區域並排印**（`Senate f47f1fd`）——
「子區域會捲」與「外層不會捲」是**兩個獨立命題**（實測兩個尺寸下外層都是 0，那是陰性讀數，不是成因）。

## ⚠ 接手的人一定會撞到的三格

1. **`--unpin` 是反向對照，不是除錯殘留** —— 沒紅過的守衛等於沒有讀數。
   本單病史就是「沒紅過卻宣告綠了」三次。
2. **`--win-size <寬>x<高>` 之前不存在** —— `SenateWindow.Run()` 一直吃得下 width/height，
   而 CLI **從來沒有傳** ⇒ 小視窗那個尺寸是**結構上驗不到**，不是「還沒驗」。
   Tim 三次截圖都是 ~300px，我三次都在 1280×800 驗。
3. **`ui --local --fold <id>` 是 toggle 不是「設成開」**，而且要**先把 fold 打開**，
   `--set` 才碰得到它裡面的欄位（收合的 Fold **不建子節點**）。
   ⚠ `--set` 一次只吃第一個，要設多格得分次呼叫（工具自己會印那行警告）。

## 📌 判準（寫給下一個要動這一層的人）

探針是「**症狀**的探針」還是「**輸入路徑**的探針」，講出來 ——
`SetScrollY` 量得到「那個容器捲不捲得動」，⛔ 量不到「滾輪事件被路由到哪」。
後者目前這棵樹上**仍然沒有任何讀數**，最終判準是真人用滾輪。
