---
id: knowhow_draft-object-drawobjectdata
topic: ucl-editor-pages
title: 3-way merge 頁加一組新設定：draft 物件 + 整組 baseline + DrawObjectData 反射繪製
type: knowhow
status: active
created_at: 2026-08-25
created_by: basecamp
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_EditorMenuPages/UCL_ScreenStreamPage.cs, Assets/Plugins/UCL_Core/Docs~/zh-Hant/API/UCL_GUILayout/UCL_GUILayout_DrawObjectData.md]
---

3-way merge 頁（UCL_ScreenStreamPage 型：config 有多個寫入端、逐欄位 baseline 判「Tim 動過沒」）要加一**組**新設定時，不必逐欄位開 baseline，也不必手刻繪製 —— 用「draft 物件」模式（2026-08-25 觀影節奏區塊首例，Tim 拍板）：

1. **page-local draft class**（`UnityJsonSerializable`），欄位名刻意 == JSON 鍵名（畫面顯示即鍵名，對帳零翻譯）；`FromConfig`/`ApplyTo` 成對放在 class 內（欄位對應只有這兩處）。預設值不重抄 —— 初值用 `FromConfig(new Config())` 從 model 流過來。
2. **merge 用整組粒度**：`SerializeToJson().ToJsonBeautify()` 當 baseline 字串（同 ocr_extra_regions 模式）。⚠ baseline 初值要是 **null 且判 null 時無條件吃磁碟**——初值設 "" 會讓「頁面預設 ≠ 磁碟值」被誤判成「編輯中」而永遠載不進來。適用前提：這組欄位的寫入端只有本頁＋手改檔；有第二個程式寫入端（daemon/Cmd 會寫）就要退回逐欄位 baseline。
3. **繪製整組交給 `UCL_GUILayout.DrawObjectData(draft, m_Dic.GetSubDic(key), name, iIsAlwaysShowDetail:true)`**（自己已有折疊 header 時 true 跳過它的標題列）。之後 config 加欄位 ⇒ 繪製零改動。List 元素實作 `UCL.Core.UCLI_ShortName`（⚠ namespace 是 `UCL.Core` 不是 `UCL.Core.UI`——2026-08-25 CS0234 實撞）給顯示名；enum-like 字串欄位掛 `[UCL.Core.PA.UCL_List(nameof(選項方法))]` 自動變下拉（Tim 提示）。
4. 驗收：round-trip 走 Cmd_Invoke 對 config 副本 Load→Save，比對「鍵集不丟、新鍵帶預設、bool 原生、共同鍵零漂移」（本例 41→45 鍵全過）。
