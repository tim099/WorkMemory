---
id: pitfall_capture-before-render-and-zero-first-frame
topic: senate-gui-bridge
title: 兩隻同形坑：截到未繪製的緩衝區／第一幀量在幀首
type: pitfall
status: active
created_at: 2026-09-15
created_by: kiara
links: []
related_docs: []
---

**兩隻都在實作當下咬到，兩隻都是「回報成功而東西沒發生」。**

## ① 截圖拍到的是還沒畫進 framebuffer 的緩衝區

`OnFrameServed` 第一版掛在 `m_Renderer.ApplyWrites(aUi)` 旁邊 —— 那裡 ImGui **還沒把這一幀畫進 framebuffer**。
⇒ 宿主在那裡呼叫 `CaptureTo`，拍到的是上一幀之前的內容。

⚠ **失效的樣子**：回 `✓ 截圖已落檔`、21396 bytes、圖打得開、看起來就是一張正常截圖。
前後兩張 **md5 一字不差**，而下拉明明開了（`--list` 的 pick 從 0 個變 3 個）。

⇒ 修法：hook 移到 `m_Controller.Render()` **之後** —— 跟原本 `--screenshot` 捕捉的**同一個位置**。
📌 那不是巧合：**能拍到的地方才是能交出去的地方。**

## ② 常駐 fps 印「第一幀 0.0 ms」

我在幀首讀碼錶，而碼錶就是那一幀剛起的 ⇒ 永遠 0。
⇒ 跟 D21 那條血證同形：**一個全綠的數字在描述一個還沒被量的東西。**
修法：第一幀的長度在**幀尾**量（跟 soak 的 `m_SoakFirstFrameMs` 同一個位置）。

## ⭐ 抓到它們的不是眼睛，是驗收條件的形狀

驗收 ③ 我寫的是「操作前後各一張截圖，**兩張不同**」。
**若寫成「截得出圖」，這一版會全綠交付。**
⇒ 判準：驗收條件寫**比較**不要寫**狀態**。狀態會過期，比較不會。
