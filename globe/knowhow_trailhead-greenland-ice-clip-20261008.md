---
id: knowhow_trailhead-greenland-ice-clip-20261008
topic: globe
title: 格陵蘭細化與冰層海岸裁切
type: knowhow
status: active
created_at: 2026-10-08
created_by: trailhead
links: []
related_docs: []
---

2026-10-08 trailhead 修格陵蘭：把粗輪廓等比縮小當冰層，會在凹海灣外留下冰；應以 land 幾何 intersection 裁冰、difference 取得外溢區，再透過 globe CLI erase，不能直接寫球面儲存。regrid 後舊像素有階梯殘點，西岸另用約4km球面緩衝清理，已看圖驗收。南美洲與格陵蘭施工區都已 done。
精細化用 Natural Earth 50m land，Disko（-55..-49.5,68..71.2）與 Scoresby（-28..-20.5,69.5..72.3）有0.06度halo；其餘保留110m輪廓。這是示意冰色，不是實測冰蓋資料。當前N4096，細筆須unit=cell，預設寬度仍按初始格換算。
重現腳本 D:/Unity/Senate/build/greenland-refine-plan.py 與 greenland-ice-clip.py；目前驗收圖 build/greenland-west-ice-clipped.png。幾何只用shapely預處理，所有繪製仍走CLI。之後動到共用交界先查目前格與最後寫入者，避免把同事新增格一起擦掉。
