---
id: knowhow_xlsx-importer-pattern
topic: hscene-editor-rework
title: HSceneAsset xlsx 匯入器三支（Sound/Texture/Character）的共用形狀：薄殼＋四道閘＋回填鍵與 Utage 路徑規則
type: knowhow
status: active
created_at: 2026-09-21
created_by: calli
links: [pitfall_npoi-multisheet-roundtrip-lossy, pitfall_npoi-editor-only-asmdef, pitfall_utage-excelparser-write-forces-xls]
related_docs: []
---

## 形狀（三支共用）

`HSceneAsset` 那側只留**反射薄殼**（`ImportXxx()` → `Type.GetType(...).GetMethod("Run")`），
實作住 `Assembly-CSharp-Editor`。理由是組件邊界不是分層美學 —— 見
`pitfall_npoi-editor-only-asmdef`。共用小工具收在 `HSceneXlsxImportUtil`
（開檔鎖預檢／`CellText`／`AppendList`／`ToUnityPath`）。

## 每支都要有的四道

1. **開檔鎖預檢**：`.~lock.<檔>#`（LibreOffice）／`~$<檔>`（Excel）存在就擋，⛔ 不放寬 `FileShare`
   —— 在別人開著的檔上寫，他一存檔就把匯入蓋掉，而兩邊都不會報錯。
   ⚠ 鎖檔是**約定不是 OS 鎖** ⇒ 偵測不到不代表沒人開著；真正的保險仍是 `FileMode.Create` 的獨佔失敗。
2. **空集不寫檔**：多種原因（資料夾空／副檔名不認得／頂層資料夾沒對應）生出同一個空集，
   而把有資料的表清成 0 筆，跟「掃描壞了」在產物上同形。
3. **dry-run 全文進 Console、確認框只給計數**（沿用 Sound 那支的四段式）。
4. **寫完回讀，逐筆比對值**（⛔ 不是比計數 —— 見 `pitfall_npoi-multisheet-roundtrip-lossy`）。

## 回填（手填欄位）

- 鍵一律**不是** Label：Label 每次會被重新推導。
  · Texture：`(Type, FileName)`
  · Character：**`(角色, Pattern)`** —— ⛔ 不可用 FileName，因為**同一張圖可以有多列**
    （`Robo/robo.png` 同時是「通常／小／大」，只有 Scale 不同）⇒ 用 FileName 當鍵會讓它們互相覆蓋。
- 回填用**排除法**（推導欄以外整列帶走），⛔ 不列舉回填欄 ——
  日後有人在表上加一欄，列舉法會靜默洗掉它，排除法會保住它。
- **預設值補在回填之後**。順序反過來會讓預設蓋掉人調過的值，而產出的表看起來完全正常。

## Utage 的三格（讀實作得到的，⛔ 不要從畫面推）

- `AdvTextureSettingData.Type` 只有 **Bg / Event / Sprite**。
  `Thumbnail` 不是 Type（它是 Texture 表 `Thumbnail` **欄**指向的圖）；Character 有自己的表。
- `AdvBootSetting.cs:148-152` 定目錄與 **defaultExt**：
  Character `.png` / BG `.jpg` / Event `.jpg` / Sprite `.png`。
- 🩸 `FileNameToPath` **沒有副檔名時會補 defaultExt** ⇒ `FileName` 一律寫副檔名。
  省略的失效樣子是**執行期讀不到圖**，不是匯入時報錯。

## 🔴 Character 表：列的順序與分組**是資料**

`AdvCharacterSetting.ParseFromStringGridRow`（:151）：
`CharacterName` 空白 ⇒ 沿用上一列的角色名；`NameText` 空白且角色名相同 ⇒ 沿用上一列。
⇒ 同一角色**只有第一列填名字**，其餘留空 ⇒ **把列重排或打散會改變資料的意思**。
寫入要依角色分組；**讀舊表也要照同一條規則把名字補回來**，
不補的話鍵會全變成 `"|pattern"`，回填一筆都對不上 —— 而那個失效樣子是「回填 0 筆」，
跟「舊表是空的」同形。

## 未量（照實標）

`Event` / `Sprite` 兩個 Type、角色子資料夾更深一層、直接放 `Character/` 根目錄的圖 ——
三條路都只有讀碼驗證，**磁碟上沒有樣本**。
