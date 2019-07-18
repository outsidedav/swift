---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

keywords: sql swift, database swift, persistence swift, data swift, orm swift, kuery swift, kitura swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 為了資料持續性使用 SQL Database
{: #sql_data}

「結構化查詢語言 (SQL)」是一種網域特定的語言，用來管理關聯式資料庫中的資料。資料持續性建議用於您的伺服器在使用時關閉的情況。若要新增資料持續性，您可以直接從 Swift 使用 SQL Database。
 

Swift 最重要的特性之一就是其類型安全。搭配使用 SQL Database 與 Swift 是合乎邏輯的選擇，因為這兩者都支援類型安全。

## 搭配使用 ORM 與 SQL Database
{: sql-orm}

使用「與物件相關的對映 (ORM)」，您可以將物件對映至關聯式資料庫，而不需要處理 SQL 陳述式。然後，您可以儲存物件以及從關聯式資料庫中擷取物件，而不需要自行執行太多剖析及序列化作業。

## 步驟 1. 開始使用 ORM
{: #start-orm}

請使用 [Swift-Kuery-ORM](https://github.com/IBM-Swift/Swift-Kuery-ORM){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 搭配 SQL 外掛程式，例如 [PostgreSQL](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 或 [MySQL](https://github.com/IBM-Swift/SwiftKueryMySQL){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")。

此範例使用 [PostgreSQL](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 外掛程式。請遵循指示以[安裝外掛程式](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")。

## 步驟 2. 將 ORM 匯入應用程式
{: #import-orm}

1. 藉由新增 `Swift-Kuery-ORM` 及 `Swift-Kuery-PostgreSQL` 套件的方式，來更新 `Package.swift` 檔案：
  ```swift
     dependencies: [
      ...
      /* Add these two lines */
      .package(url: "https://github.com/IBM-Swift/Swift-Kuery-ORM.git", from: "0.0.1"),
      .package(url: "https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL.git", from: "1.0.0"),
    ],
    targets: [
      .target(
        name: ...
        /* Add these two modules to your target(s) dependencies */
        dependencies: [..., "SwiftKueryORM", "SwiftKueryPostgreSQL"]),
    ]
  ```
  {: codeblock}

2. 若要新增 ORM 功能，請在 `Application.swift` 的開頭處，新增下列 `import` 行：
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## 步驟 3. 建立資料庫
{: #create-db-sql}

1. 在您機器上設定 PostgreSQL 之後，使用終端機來建立資料庫：
  ```
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. 起始設定 `Application.swift` 中的資料庫：
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **附註**：對於並行要求，系統會使用連線儲存區。

## 步驟 4. 定義模型
{: #define-model-sql}

1. 建立模型 `Grade`：
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. 然後，建立您的表格：
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    /* Error */
  }
  ```
  {: pre}

## 步驟 5. 管理資料
{: #manage-data-sql}

### 儲存資料
{: #save-data-sql}

若要儲存 `Grade` 的實例，請建立一個實例，然後呼叫 `save()`：
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### 尋找資料
{: #find-data-sql}

若要從資料庫中擷取所有 Grades，您可以使用靜態呼叫 `findAll()`：
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### 更新資料
{: update-data-sql}

更新資料庫中的 Grade 也使用類似的方法：
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### 刪除資料
{: #delete-data-sql}

刪除資料庫中的 Grade 也使用類似的方法。
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

這些呼叫全部都採用呼叫一次的處理程式，並在完成資料庫呼叫時執行該處理程式。

## 搭配使用 ORM 與 Kitura
{: #kitura-orm}

為了更容易試用 ORM，[FoodTrackerBackend 指導教學](https://github.com/IBM/FoodTrackerBackend){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 可以從 iOS 應用程式中儲存並提取 Meal 物件，並直接放進 PostgreSQL 資料庫中。即使您已完成指導教學，也值得您再試一次，以瞭解 Swift-Kuery-ORM 的強大功能，以及它如何簡化您的 Kitura 程式碼。

## 直接使用 Swift-Kuery
{: #swift-kuery}

如果因為您需要對資料庫有更多的控制權，而受到 ORM 的限制，您可以使用 SQL 抽象層 [Swift-Kuery](https://github.com/IBM-Swift/Swift-Kuery){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")，在其中您可以建立 SQL 查詢。
