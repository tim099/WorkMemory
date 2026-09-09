---
id: pitfall_scrub-entry-and-window-driving
topic: hscene-editor-rework
title: Scrub 的入場當場觸發＝憑空挑方向；m_SlideInterval<=0 的「每幀判定」＝門檻永遠過不了
type: pitfall
status: active
created_at: 2026-09-09
created_by: calli
links: []
related_docs: [Assets/Scripts/UCL_Assets/ClickTypeAsset.cs, Docs/API/UCL_Asset/ClickTypeAsset.md, commit:532675ef3]
---

`ClickTypeAsset.CheckContinuous` 的**入場分支**（`!ioState.active` 那一格）同時做兩件事：初始化窗口 **並且回 `true`（當場觸發一次）**。而那個「當場觸發」對不同型別的語意完全不同 —— 2026-09-09 Tim 兩則訊息各修掉一格。

## ① 入場當場觸發 × Scrub ＝ 方向被憑空挑一個

**症狀**：`ScrubY` ＋ `PingPong2`，**滑鼠剛按下的第一次拖曳即使往下拖，Flag 值仍然增加**（後續正常，只有起始那一格錯）。

**鏈路**（逐段讀過，不是推的）：
```
入場 → lastDirSign = 0 → ContactService.GetScrubDir 回 0
     → Cycle(PingPong2, iScrubDir:0) ＝「照舊走環」→ PingPong2 預設往上 → 值 +1
```
⇒ 原註解「入場這一次還沒有窗口位移 ⇒ 方向未知（**照舊前進**）」——
**前半是真的，後半就是那個 bug**：方向未知時不該挑一個方向前進。

**修法（Tim 拍板）**：Scrub 入場**一律 `return false`**，方向與觸發都交給第一個窗口。
📌 我第一版是「入場從 `clickInfo.dragDir` 取軸向」（`dragDis = GetDragDis(dragDir)` ⇒ 讓它通過門檻的位移向量就在手上）。
⚠ **他那版才對**：取 `dragDir` 是替使用者**推測**一個方向，而不觸發是**承認那一刻還沒有方向**。
⇒ 判準：`Slide` / `Hold` 不看方向 ⇒ 入場即成立沒有語意問題；**Scrub 沒有方向就沒有「這一次要往哪推」**。

## ② `m_SlideInterval <= 0` 原本是「每幀判定」，而那等於門檻永遠過不了

**症狀**：沒填判定間隔時，手一直滑而 Flag 一格都不動。

**成因**：那一步原本「**判完就開新窗口 —— 不論觸發與否**」⇒ 每幀重設 `windowStartPos`
⇒ `aDelta` 只有一幀的位移 ⇒ `m_SlideDistance` 基本不會過。
⚠ 失效樣子是「完全沒反應」，跟「條件不成立」「資料沒填」**全部同形**。

**修法**：Scrub 在該設定下改走**位移驅動** —— 沒過門檻就**保留窗口起點繼續累積**，過了才觸發並開新窗口。

| `m_SlideInterval` | 窗口怎麼關 | 適用 |
|---|---|---|
| `> 0` | 時間驅動（判完就開新窗口，不論觸發與否） | Slide / Hold / Scrub |
| `<= 0` | **位移驅動**（每滑過一個 `m_SlideDistance` 推一格） | **只有 Scrub** |

⛔ **Slide / Hold 不走位移驅動**，維持原行為與原 warning。
理由：`Hold` 的判定是「**沒動**超過門檻才算」，位移驅動對它沒有意義 —— 那條路要另外想，別順手塞。

## 📌 給下一個動這支的人

- `ContinuousState` 的四個欄位（`active` / `timer` / `windowStartPos` / `lastEvalFrame` / `lastDirSign`）**per-state 不是 per-group**
  ⇒ Slide / Hold / ScrubX / ScrubY 各一份，互不干擾。那是 `ContactService.ContectGroup.GetContinuousState`
  （2026-09-09 新增的 ref-return）唯一分派的那個一對一。
- **`lastDirSign = 0` 有兩個來源，而語意不同**：自動播放明確表態（`GetScrubDir` 之外傳的 0）／窗口還沒量到方向。
  ⚠ 混在一起就是 ① 那個 bug。
- 入場門檻 `CheckGates` 用的是 `clickInfo.dragDis`（**兩軸合成**），而 Scrub 的觸發只看**單軸**
  ⇒ 兩者本來就該分開問。`ScrubY` 遇到純橫拖時 `dragDis` 可能已過門檻而縱軸一格都沒動。
