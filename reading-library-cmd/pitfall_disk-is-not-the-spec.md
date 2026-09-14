---
id: pitfall_disk-is-not-the-spec
topic: reading-library-cmd
title: 磁碟不是規格 —— 換寫入端的對拍對象是舊 writer，而被 git autocrlf 洗過的軸不歸 writer 管
type: pitfall
status: active
created_at: 2026-09-14
created_by: calli
links: [reading-library-cmd/pitfall_bytewise-diff-not-axis-list]
related_docs: [task:TASK-0166, ucl_core:Assets/Plugins/SCP_Core/Runtime/Json/SCP_JsonStyle.cs]
---

**「跟磁碟逐位元組相同」不是驗收條件 —— 磁碟是沉積，不是規格。**

換寫入端時，「搬對了」的對象是**舊 writer**，不是磁碟上那堆檔。
拿磁碟當規格會得到一個**永遠達不到、而且會把你往錯方向推**的目標。

## 🩸 血證（2026-09-14 calli，TASK-0166 ①）

`BookNotes/Library` 596 份 JSON，逐位元組跑 `SCP_JsonWriter` 三種版面：

| 版面 | 與磁碟相符（行尾無關） |
|---|---:|
| `Default`（SCP 預設） | 5 |
| 只改冒號空格（＝我 09-11 的修法） | 125 |
| `UclLegacy`（四根軸都對齊） | **426** |

⇒ 剩下的 170 份對不上**不是 bug**：那是**另外兩支 writer** 的沉積（2 空格那批、tab＋冒號有空格那批）。
一個資料夾裡住著三個時代的產物，**沒有任何單一 writer 能同時對上全部**。

## ⚠ 而最會騙人的是行尾那一根軸 —— 它整根不歸 writer 管

`AgentCommands` repo 是 `core.autocrlf=true` ⇒ **checkout 會把整份換成 CRLF**（含結尾那一個 LF）。
⇒ 工作樹上的行尾是 **git 的產物**，不是任何 writer 寫出來的東西。

我一度讀了舊呼叫端那一行（`File.WriteAllText(path, ToJsonBeautify() + "\n")`），
照它模擬「本體 CRLF ＋ 結尾裸 LF」—— **相符份數從 269 掉到 157**。
📌 我抄對了舊 writer，卻抄錯了規格，因為那一根軸在我量它之前就被 git 改過了。

## 動作型判準

1. **換寫入端的對拍對象是舊 writer 的實作**（去讀 `SerializeValueBeautify` 那種函式本體），
   ⛔ 不是磁碟上的樣本。磁碟只能拿來**間接驗**「我有沒有把舊 writer 抄對」。
2. **量任何檔案格式之前先問 `git config core.autocrlf` 與 `.gitattributes`。**
   被 git 正規化過的軸沒有資格進你的規格表 —— 而它在磁碟上跟真軸長得一模一樣。
3. **相符份數印成讀數，⛔ 不要當通過條件。** 沉積型資料的相符率會隨磁碟變動；
   拿它當閘 ⇒ 永遠紅，而紅得沒有資訊。真閘要用**一份自己寫死的期望字串**
   （一次蓋掉所有軸），那份字串抄自舊 writer 的實作。
4. 格式的旋鈕**收成一個具名型別**（`SCP_JsonStyle`），⛔ 不散成 writer 的四個 bool ——
   散了之後下一個呼叫端只會抄到它看得見的那幾格，而漏掉的那幾格寫出來照樣是合法 JSON。

## 📌 與 [[pitfall_bytewise-diff-not-axis-list]] 的分工

那一條說「軸表會漏」；這一條說**連對的軸表也可能在量一個假的東西**。
09-11 我漏了兩根（陣列括號位置／空容器渲染，而那兩根我**命名過**只是沒放進表）；
09-14 補齊之後才發現第六根根本不是我的。⇒ **名字有了不等於量了；量到了不等於它歸你管。**
