---
id: pitfall_measurement-and-lifetime-traps
topic: senate-bank-rebuild
title: 六隻：log 接在別人的生命週期上／自動啟動鎖住 build／量具掛住被讀成 bug／對照組名不副實
type: pitfall
status: active
created_at: 2026-09-14
created_by: basecamp
links: []
related_docs: [commit:c57936a, task:TASK-0209]
---

**這一段咬我的，沒有一隻是「功能壞了」—— 全是「要查的時候那東西剛好不在」或「量具在說謊」。**

**① 接在別人生命週期上的日誌，會跟著那個人一起死。**（A5，連錯兩版）
- v1：4 顆同時 spawn 共用一份 `_server_start.log` ⇒ `WriteAllText` **互相截斷覆蓋**。
- v2：改成一顆一份（檔名帶 **parent** pid）⇒ 4 份檔、4 份表頭都在，
  **而輸掉單例鎖那三顆的訊息仍然是 0 筆** —— 輸出接成 parent 的管線，parent 跑完就退，
  非同步讀取器跟著死。
- v3（採用）：**child 自己 tee**，parent 完全不 redirect ⇒ 「拿不到單例鎖」留住 3 筆。
⇒ **落檔的責任歸產生那些字的那個 process，不歸拉起它的那個。**

**② 自動啟動讓「build 被鎖」從偶發變成常態。**
`ServerHost` 檔頭第 ③ 條血證（exe 會被常駐的自己鎖住 ⇒ build 前先 stop）射程變大了：
以前要人刻意 start；**現在任何一支委派 Cmd 都會留下一顆**。
`build.sh` 早就會先 stop，**但開發時的 `dotnet build` 不會** ⇒ 撞 `MSB3027 檔案鎖定者`。
⚠ 而更難的一次是：我停了兩顆、再 build、又撞第三顆 —— **以為是殘留，其實是我自己一個
逾時被移到背景的測試腳本還在跑，每輪都拉起一顆**。
📌 一般形：**「清不乾淨」與「有人一直在製造」在畫面上同形**，而處置相反（再清一次／去關源頭）。
抓到它的是去查那個 pid 的**命令列**，不是再停一次。

**③ 我差點把自己的 shell pipeline 讀成產品 bug。**
`timeout 90 senate cmd bank … | grep -aE …` 掛住不輸出，而 Server log 明明寫著 `✓ bank`。
我當下寫下的是「Server 跑完了而 CLI 看不到結果 —— 這可能是**真 bug**」。
拿掉 grep 直接跑：**`exit=0`，一切正常**。掛住的是我的管線。
📌 **量具的失效，跟被量的東西壞掉，在畫面上同形。**

**④ 對照組的名字不是它的行為。**
B6 我第一版寫了一個叫 `UnsafeDebit` 的「反向對照」，繞一圈之後**還是呼叫了有鎖的 `Debit`**
⇒ 它會全綠，而那個綠什麼都沒證明。名字叫 Unsafe，讀起來就像已經在測不安全的路徑。
⇒ 真正的作法：**把鎖從 code 裡拿掉、重建、重跑、再從備份還原並用 md5 驗**
（A3 與 B6 都是這樣紅過的：4 顆全登記成功／餘額 −100）。

**⑤ 框架的守衛擋過我一次，而那道守衛值得知道它在。**
`Cmd_Bank` 讀了 `caller`／`cmd_id` 卻沒宣告在 `ArgSpecs` ⇒
`InvalidOperationException: Cmd 取了一個自己沒宣告的參數 —— 規格與實作不同步`。
⚠ 沒有它的話，`caller` 會靜默是空字串，而**錢就變成沒有人簽名的**。
📌 對照：`senate ucmd` 那條路**沒有**這層（未知參數靜默取預設值）——
同一個系統裡，寫入端的守衛與派遣端的守衛不是同一格。

**⑥ 新目錄的 `.meta` 要自己生。**
`SCP_Core` 有 149 個 `.meta` 入版控，而我在 Unity 外面新建的 `Runtime/Bank/` 沒有。
不生的話 **GUID 由「誰先在 Unity 開啟哪一份工作副本」決定**，而它同時掛在多個消費端底下
⇒ 兩個人各自開就是兩顆 GUID，那時參照會斷而沒有任何一層說得出為什麼。
