---
id: decision_auto-register-taxonomy
topic: senate-gui-page-catalog
title: 自動收頁的判準與四類別界線（Tim 2026-09-22 拍 C）
type: decision
status: active
created_at: 2026-09-22
created_by: summit
links: []
related_docs: []
---

Tim 2026-09-22 拍板 **C**：頁面目錄從顯式 `Register` 改成**反射自動收頁**，判準是**繼承 `SCP_GuiPage`**，⛔ **不看建構子**。
逐條收窄：① 判準是繼承　② 排除靠標記　③ 身分以 `TypeFullName` 為基準、key 撞名**必須在 build 階段報錯**　④ 下拉標籤 `Key(TypeName)`，相等時只印 `TypeName`。

## 四種「這頁不要出現」，各有各的機制（⛔ 沒有第五套）

| 類別 | 用什麼 | 收錄 | 入口頁列 | `Create` / `--page` |
|---|---|---|---|---|
| 根本不是頁（測試探針） | `[SCP_PageIgnore("理由")]` | ❌ | ❌ | ❌ |
| 彈窗／選項頁（內容由母頁決定） | `MenuGroup => null` | ✅ | ❌ | ✅ |
| 入口頁自己／只從工具列進的頁 | `MenuGroup => null` | ✅ | ❌ | ✅ |
| 給人繼承的基底頁 | **`abstract`**（語言自帶，編譯期擋 `new`） | ❌ | ❌ | ❌ |

## 🩸 我差點翻掉一個舊拍板

我提議「照 UCL 的 `ShowInPageMenu`（bool）讓頁面自己決定顯示」，理由是「`MenuGroup` 兼差當 opt-in ⇒ 想列在未分組做不到」。
⇒ **D16 ①（2026-08-23，Tim）**：`ShowInPageMenu` 正是**被 `MenuGroup` 取代掉**的那個，
而「未分組做不到」D16 早就解了（**空字串 ≠ null**：空字串＝列進去、沒分組名）。
📌 擋下我的不是我更仔細，是 Tim 一句「之前好像就是拍板用這個機制」。
⇒ **提議改一個既有機制之前，先 grep `Docs/Logs/Decisions.md`。**

## 為什麼要把 context 遞進去（而 UCL 那套不必）

UCL 的 `UCL_EditorMenuPage` 走 `Activator.CreateInstance(t)`（**無參**）—— 頁面自己去全域拿資料。
而本層刻意**沒有**那個全域：**D13 ①-1** 逐字「沒有 `Ins` 單例…留著 singleton 的症狀不是崩潰，是開第二個視窗之後兩邊互相蓋」。
⇒ 補法是**生的時候遞給它**（13 支頁一個字都不用改，D13 不用翻），
⛔ 不是把頁面改成「先生出來再塞」—— 那會長出新的失效態「還沒塞就被畫」。

## Tim 的前提（⚠ 它會過期）

逐字：「**後台是我在使用的操作介面，有漏頁面或開不起來我會發現。**」
⇒ 因此**不另造**紅字機制、不為探針誤入選單加第二道防線。缺陷落進既有的 `Diagnostics`，入口頁畫出來就夠。
⚠ 哪天後台多一個使用者、或它被塞進 CI，這幾格要重新評估。
⛔ 而「安靜給錯值」那族（key 撞名 ⇒ 同一行指令開到另一頁）**他看不出來** ⇒ 那格照樣要機械擋。
