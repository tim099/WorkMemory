---
id: decision_registry-three-states
topic: library-media-migration
title: registry 的三種 state（migrated / kept_archive / born_new）與第一筆刻意不遷的拍板
type: decision
status: active
created_at: 2026-09-17
created_by: summit
links: []
related_docs: [task:TASK-0171, commit:b681524, commit:0bbbd0a]
---

## 拍板：registry 的 `state` 現在有三種被程式讀

（summit 2026-09-17，Tim「171 全包 GO」授權自決；程式面 SCP_Core `b681524`，資料面 BookNotes `0bbbd0a`）

| state | 意思 | 讀取端怎麼處置 |
|---|---|---|
| `migrated` | 這筆 Archive 的內容已進正本 | `op=scan` **預設隱藏**（重複資料） |
| `kept_archive` | **刻意不遷**，Archive 就是它的正本 | ⛔ **不隱藏**；列在新的 **A′** 節並印理由（不是候選） |
| `born_new` | 新流程直接在 Library 建的，**不經遷移** | 不影響 Archive 掃描；它讓帳回答「資料現在在哪」 |

⇒ 原本只認 `migrated` ⇒ 帳本記的是**誰用過 migrate**：
「刻意不遷」與「還沒遷」同形、「新流程直接建」在帳上根本不存在。

### 第一筆 `kept_archive`：`Archive/xiaoyuehan-qipa-xiaoguo`（奇葩小國）

不併進正本。理由是**兩邊章號是兩條不同的軸**：Archive 的 `chNN` 是**閱讀順序**（集號寫在標題裡，
ch34＝奇葩小國05），Library 的 `chapter_id` 是**該系列的集號**（`0001`＝第 01 話）。
併＝逐章把人寫的標題重新定序成集號，錯一章的失效樣子是「正本多一章、集號與內容對不上」而**不會叫**。
`reopen_condition` 寫在那筆紀錄裡（要併就另開單：逐章對集號、每章標 source、用既有 `gap` 欄位表達洞）。

⚠ 開單描述有兩處與磁碟不符，已在單上更正：① Archive 那批是**根層 11 章**（ch33–ch42，含兩個 ch36 變體）
＋ branch `ame` 2 章 ＋ 4 張人物卡，不是只有 2 章＋4 卡；
② **「最大章號＝章數、不准有洞」對這個 store 不成立** —— `chapter.json` 有 `gap` 欄位
（`SCP_LibraryIO.Key_Gap`），活體是 `series-qi-pa-xiao-guo/readers/summit/chapters/0006`。
