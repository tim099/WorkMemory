---
id: knowhow_kitchen-meals-ingredients-1013
topic: blocksail-production
title: 城鎮廚房28道菜與實際食材分列的資料邊界
type: knowhow
status: active
created_at: 2026-10-27
created_by: Sirius
links: []
related_docs: []
---

TASK-1005 與 TASK-1013 已完成。城鎮廚房資料仍在 world/food.js：HOME_RECIPES 為實際複合配方，HOME_MEALS 只替 eat:<貨> 記錄提供菜名與圖。homeMenu() 輸出11道複合菜＋17道直接餐點，共28道；globe.html 自動帶給 Blockside 89_townui.js，新增菜不必另改核心或重打包。

Tim 拍板：食用餐點（份）與消耗食材（桶）分成兩部份。實際食材記在 foodDay.ingredients、serveDay.mealIngredients，再保存到 fed.ingredients.home/service；含加餐、免費供應及帶meal的服務供餐，排除日用與單獨飲品。不能從理論配方倒推，類別要記實際扣掉的貨物。舊紀錄缺欄位顯示清楚提示，下一天開始累積明細。

新增菜會合理改變消費選擇。tools/home_menu_check.cjs 的四情境14天對照使用同一份舊菜單確認引擎沒變，再恢復現行菜單驗UI；別把它解讀為「新增菜後經濟不變」。tools/home_recipe_check.cjs 逐菜驗實際市場扣料、金錢守恆、16缺必要食材情境與野味替肉；食材熱量不得小於產出，半份向下取整。圖表工具 tools/dishes_check.cjs 現對帳31張菜圖。

權威指路：docs/RESOURCES.md §5/§9、docs/TOWN_UI.md §4.4；驗收與完整生圖提示詞在 docs/evidence/home-menu1005/ 和 home-expand1013/。6張新菜先在ArtGallery 58fba52展出再匯入，遊戲主線 beccc08；TASK-1005主線5955120。畫風參考本批實際使用的ArtGallery/RawImages/sirius_blocksail_home1005_stewed_vegetables_master.png，白底原稿保留，遊戲128RGBA、104px主體、alpha0/255、32px白深底親看。兩次worktree已清理，沒有待接的施工場。
