---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用 SQL 数据库实现数据持久性
{: #sql_data}

结构化查询语言 (SQL) 是一种特定于域的语言，用于管理关系数据库中的数据。建议您使用数据持久性，以应对服务器在您使用期间关闭的情况。要添加数据持久性，可以直接通过 Swift 使用 SQL 数据库。Swift 中最重要的其中一个功能是其类型安全。将 SQL 数据库与 Swift 配合使用是符合逻辑的选择，因为这两者都支持类型安全。

## 将 ORM 用于 SQL 数据库
{: sql-orm}

通过对象关系映射 (ORM)，可以将对象映射到关系数据库，而不必处理 SQL 语句。然后，可以在关系数据库中存储和检索对象，而无需亲自执行大部分解析和序列化工作。

## 步骤 1. ORM 入门
{: #start-orm}

将 [Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM) 用于 SQL 插件，例如 [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) 或 [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL)。

对于此示例，将使用 [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) 插件。遵循指示信息在[此处](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation)安装插件。

## 步骤 2. 将 ORM 导入应用程序
{: #import-orm}

1. 通过添加 `Swift-Kuery-ORM` 和 `Swift-Kuery-PostgreSQL` 包来更新 `Package.swift` 文件：
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

2. 要添加 ORM 功能，请在 `Application.swift` 开头添加以下 `import` 行：
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## 步骤 3. 创建数据库
{: #create-db-sql}

1. 在计算机上设置 PostgreSQL 后，使用终端来创建数据库：
  ```
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. 在 `Application.swift` 中初始化数据库：
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **注**：连接池用于并发请求。

## 步骤 4. 定义模型
{: #define-model-sql}

1. 创建模型 `Grade`：
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. 然后，创建表：
  ```swift
do {try Grade.createTableSync()
  } catch {
    /* Error */
  }
  ```
  {: pre}

## 步骤 5. 管理数据
{: #manage-data-sql}

### 保存数据
{: #save-data-sql}

要保存 `Grade` 实例，请创建实例，然后调用 `save()`：
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// 保存到数据库
grade.save { student, error in
  ...
}
```
{: pre}

### 查找数据
{: #find-data-sql}

要在数据库中检索所有 Grade，可以使用静态调用 `findAll()`：
```swift
// 获取数据库中的 Grade 的数组
Grade.findAll { students, error in
  ...
}
```
{: pre}

### 更新数据
{: update-data-sql}

使用类似方法来更新数据库中的 Grade：
```swift
grade.grade = 80

// 更新 Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### 删除数据
{: #delete-data-sql}

使用类似方法从数据库中删除 Grade：
```swift
// 删除 Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

所有这些调用都将采用调用一次的处理程序，并在数据库调用完成时运行该处理程序。

## 将 ORM 用于 Kitura
{: #kitura-orm}

要更轻松地试用 ORM，请完成 [FoodTrackerBackend 教程](https://github.com/IBM/FoodTrackerBackend)。FoodTrackerBackend 可以将 iOS 应用程序中的 Meal 对象直接保存并访存到 PostgreSQL 数据库中。即使完成了本教程，也值得再次浏览本教程以了解 Swift-Kuery-ORM 的能力，以及它是如何简化 Kitura 代码的。

## 直接使用 Swift-Kuery
{: #swift-kuery}

如果您需要对数据库有更多控制权，但 ORM 限制了您这样做，那么可以使用 SQL 抽象层 [Swift-Kery](http://github.com/IBM-Swift/Swift-Kuery)，在其中可以进行 SQL 查询。
