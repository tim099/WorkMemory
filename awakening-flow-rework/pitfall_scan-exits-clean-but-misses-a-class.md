---
id: pitfall_scan-exits-clean-but-misses-a-class
topic: awakening-flow-rework
title: 掃描乾淨 exit 0，而缺的那一類不會出現在結果裡（管線／正則／exit code 三種變體）
type: pitfall
status: active
created_at: 2026-08-13
created_by: basecamp
links: []
related_docs: []
---

**掃描跑得完、乾淨地 exit 0，而缺的那一類不會出現在結果裡。**（by: summit 命名，2026-08-12）

今天同一族栽三次，三次的共同形狀是「**我量的那個東西，不是我要的那個東西**」，而且**每次都正常結束**：

| # | 幹了什麼 | 缺的那一類 | 怎麼發現的 |
|---|---|---|---|
| 1 | `tavern_catchup.py \| head -32` | head 關管線 → 工具 broken pipe 死在收尾前，**「✓ cursor 推進到」從沒跑** | 酒保對 Tim 發假警報「basecamp 可能死了」（cursor mtime 是它的已讀信號） |
| 2 | `grep 'p\["欄位"\]'` 找必要欄位 | **只吃雙引號** → f-string 內 `p['agent']` 整類隱形；含 `=` 的行被排除規則連讀取一起濾掉 | summit 用 AST 複驗，5 欄 → 7 欄 |
| 3 | （summit 版）`python … \| tail -12; echo EXIT=$?` | 拿到的是 **tail 的退出碼**，不是 python 的 | 她自己起疑重測，真值是 2 |

**⇒ 可執行的三條**：
1. **會寫檔／推游標的工具，不准接 `head`** —— 要少讀就 `> file` 再讀，或用 `tail`（tail 讀完整條串流，不觸發 SIGPIPE）。
2. **要量「程式怎麼讀一個欄位」，用 AST 不用正則** —— 正則不懂語法，它只懂字元。`ast.Subscript(ctx=Load)` 一次到位。
3. **pipeline 裡量 exit code 一定錯** —— `$?` 是最後一個指令的。要真值就別接管線（或用 `PIPESTATUS`）。

⚠ **修法不是「下次更仔細」** —— 那是願望。要嘛換懂語法的工具，要嘛**把 pattern 的覆蓋率本身當成一個要驗的東西**。
