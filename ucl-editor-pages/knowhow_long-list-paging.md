---
id: knowhow_long-list-paging
topic: ucl-editor-pages
title: 長清單分頁走 DrawSelectPage（每頁 10 筆）＋ 三個撞過的細節
type: knowhow
status: active
created_at: 2026-08-21
created_by: basecamp
links: []
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/API/UCL_GUILayout/UCL_GUILayout_Overview.md, Assets/Plugins/UCL_Core/UCL_Core_Scripts/UICore/UCL_GUILayoutPopup.cs:112, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_RelationshipPage.cs]
---

長清單（事件流／訊息／紀錄）分頁一律走 `UCL_GUILayout.DrawSelectPage(dic, itemsCount, maxItemsPerPage)`，每頁 10 筆 —— `DrawList` / `DrawDictionary` / `DrawHashSet` 內部用的就是它，手刻清單再刻第二套翻頁列＝同一個專案兩種操作習慣。用法與四個行為細節寫在文件 §5.4（見 related_docs），這裡只記三個「文件上看不出來、要撞過才知道」的：

① **翻頁狀態的容器要跟 `PopupSearchCache` 分開**（同折疊狀態那條血證）：資料重載路徑上的 `Clear()` 會把頁碼一起清掉，症狀是「翻到第 3 頁按個 Refresh 就跳回第 1 頁」，看起來像 UI bug 而不像 key 撞名。

② **它只 clamp 不 reset** ⇒ 切換「在看誰／哪一組資料」時要自己 `GetSubDic(<key>).Clear()`。2026-08-21 `UCL_RelationshipPage` 實測：A 的第 4 頁切到同樣有 4 頁的 B，停在 B 的第 4 頁 —— 那不是使用者翻到的，而它跟正常翻頁**完全同形**。

③ **改成分頁的同時要把「預覽 N 筆＋展開全部」那種二態拿掉。** 那個設計在 37 筆時只有「太少」與「太多」兩檔，留著就變成兩套並存的瀏覽方式（而使用者會問哪個才是對的）。

⚠ 併發注意：`DrawSelectPage` 只有 `itemsCount > maxItemsPerPage` 時才畫翻頁列並回傳非零 startIndex，所以呼叫端**不能假設它一定畫了東西**（少量資料時它不佔任何 layout 位置）。
