---
id: pitfall_stall-attribution-is-not-causal
topic: unitask-editor-async
title: 斷拍歸因欄不是因果：兩條排除（沒有 cmd 行／斷拍先開始）後 151 筆剩 0
type: pitfall
status: active
created_at: 2026-09-10
created_by: summit
links: []
related_docs: []
---

`_cmd_slow.jsonl` 的 `kind=stall` 那條路，**歸因欄不可以直接當因果**。

`overlapping_cmds` 收的是**時間重疊**。實測（2026-09-10，200 筆斷拍／累計 725s／最長 47s）：
「唯一嫌疑犯」151 筆，加上兩條排除之後 **剩 0**。

⛔ 兩條排除（都用檔裡**現有**欄位，不必改量具）：
1. **那支沒有 `kind=cmd` 行** ⇒ 它 handler 與 runner **兩個數字都 <1000ms**
   （門檻是 `if (aMs < CMD_SLOW_MS && iRunnerMs < CMD_SLOW_MS) return;` ——**各判一次**）
   ⇒ 它撐不起一次 >1s 的連續斷拍。**139/151 是這種。**
2. **`stall.stalled_since` 早於 `cmd.started_at`** ⇒ 斷拍先開始，那支還沒起跑。**12/151 是這種。**

🩸 而這條沒寫下來的代價是可數的：我照沒排除的讀數排了一份「熱點排行」，
榜首 `Tavern/post`（22 次／68.2s）**零個 cmd 行** —— 我把那個缺席讀成「慢而未量」，
據此對外宣告兩次「量具的括號有洞」（那個洞 09-07 就補了），還替它寫好了修法。
⇒ **缺席的真意是「它兩個數字都很快」。**

📌 而榜首之所以是它，只因為 `post` 是**最頻繁**的 cmd（每次 commit／留言／公告都發）
⇒ **這把尺量的不是誰慢，是誰最常在場。**

⚠ 它治不了的那半：**「同時發生」與「造成」本量具分不出來** ——
排除法只產出「沒有嫌疑犯」，不產出「找到兇手」。要真的抓主緒兇手需要**斷拍區間內的主緒堆疊**，
而那不在現有欄位裡（TASK-0161 留言已記，Tim 拍板不做）。
