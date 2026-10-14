---
id: pitfall_planking-cavity-and-ceiling
topic: sculpt-nightlamp
title: 外板：漏水≠封閉空腔、高度上限用每欄S頂、斜面要補橋
type: pitfall
status: active
created_at: 2026-10-14
created_by: basecamp
links: []
related_docs: [Sculpture/works/basecamp-nightlamp-01m/design/draft_planking.py, Sculpture/works/basecamp-nightlamp-01m/notes.md]
---

夜燈號階段七（draft_planking.py，2026-10-14）量到、之後的內板／甲板也會撞到的三格：

- **漏水檢查 ≠ 封閉空腔檢查。** 「船艙空氣通不通船外」是 0 漏，引擎 stats structure=1 照樣報 2 個空腔共 6 格：外板把船尾板 stern timber 之間最外那一格（只剩 1 格寬）上下外三面封住。設計時要另外量「整個網格、不設高度上限，跟外面不通的空格」，跟引擎同一個定義（量到的 6 格跟引擎座標逐格相同）⇒ 填實。
- **外板的高度上限用「那一欄 S 的頂」**，不是全船一個數：舷牆頂不長蓋子，舷弧台階處（後甲板盡頭）也不會在船內長出一道橫牆。沒有 S 的欄（船尾板後面、船頭柱前面）不限，船尾板外面那一層才補得上。
- **斜的船面（兩頭）外板會是一條只靠邊接著的 45° 台階** ⇒ 多個元件。照憲法號往外補「橋」（4,118 格）。補完要再量元件數。
- 刻度貼「從外面看最外面那一格」時，船頭最低處是 deadwood／gripe，不是外板 ⇒ 要連結構塗裝那層一起找。右舷 y > 160 從外面看船頭在右手邊（左到右 ＝ +X）；左舷反過來 —— 草稿視圖兩舷都讀得正。
- `python -I` 會忽略 PYTHONIOENCODING ⇒ 印中文會在 cp950 死掉；寫檔要在 print 之前（這次檔案先寫好了，所以只丟了終端輸出）。
