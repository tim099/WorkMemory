---
id: pitfall_wheel-lids-water
topic: sculpt-matsushima
title: 共用舵輪只有輪、長砲管的 house 餘量、蓋板擋水住在 25_doors.js
type: pitfall
status: active
created_at: 2026-10-23
created_by: ridge-001
links: []
related_docs: []
---

- 共用雙舵輪 basecamp-frigate-constitution-01m-wheel 只有輪：座 10-16 拆成 -wheel-stand（TASK-0640）⇒ 只掛輪會浮空（松島號）。帆船掛共用木頭座；鋼鐵艦用 ridge-001-matsushima-01m-wheel-stand 那種鑄鐵座（Tim「其他船的舵輪支架跟松島號不同」）。
- 砲門蓋板：12 cm 速射砲砲管長，basecamp portlids.py 的 house 餘量 0.5 不夠（遊戲匯入檢查 P5／S5 ✗）⇒ HOUSE_MARGIN 1.5。
- 🩸 本小姐一度說「蓋板不擋水」：只在 33_*.js grep portlid。擋水寫在 25_doors.js（portsFloodInit／portsSurvey，照 62_ports 建的蓋板清單）—— 有蓋板的砲口開局隔水、打壞才進水。查功能先找讀那份資料的人，別只 grep kind 字串。
- 船首樓突出砲台的 57 mm 射口不做蓋板（Tim 交給本小姐判斷）：照實留成進水口。
- 轉了非直角 z 角的模型，匯出時角度烤進頂點、座標捨到 0.01 ⇒ 要把網格當格子用（shellVox、平滑）得先轉回軸對齊（62_msmooth.js msmAlign，邊角度的眾數，⛔ 不是平均）。
