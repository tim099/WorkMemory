---
id: pitfall_retire-cmd-not-single-writer
topic: unity-senate-migration
title: Cmd 退場不等於單一寫入端：Unity 頁面自己也寫
type: pitfall
status: active
created_at: 2026-09-28
created_by: gura
links: []
related_docs: []
---

**Unity 端「Cmd 退場」≠「這份資料只剩一個寫入端」—— Unity 的頁面常常自己也寫。**

- 2026-09-28 TASK-0328：`Tavern op=createroom／join／leave` 退場後，全樹一掃，`UCL_ChatTavernPage`（Unity 酒館頁）自己還會 `CreateRoom`／`AddMember`／`RemoveMember`／`GetOrCreateIdentity` ⇒ 那幾支 IO **不能跟著刪**，這一頁歸 TASK-0326 分析。
- ⇒ 搬一塊「寫」的時候，判準是 **grep 那支 IO 的全部呼叫端**（含 EditorMenuPages），不是只看 Cmd 的 op 表。
- 退場手勢（兩處都用了）：op 名留在分派表、規格留空（`new UCL_CmdOpSpec()`，Known 空 ⇒ 不驗參數）⇒ 舊呼叫一定走到「搬到哪／為什麼廢棄」的指路，⛔ 不會先被參數檢查擋成別的錯、也不會變成「未知 op」。
- 動錢那塊的對照組：`UCL_TreasuryRequestStore` 全樹只剩 `Cmd_Treasury` 在用（`Approve` 早就零呼叫端）⇒ 可以整支刪。**兩塊結論不同，是 grep 出來的，不是推出來的。**
- 還沒搬的相關項：`create_trpg_room`（還往已經沒人讀的 `notify_config.json` tavern_mirror.rooms 登記；TRPG 開房應改成「建房＋設 TRPG 分類」）；en／ja／zh-Hans 文件仍指舊指令。
