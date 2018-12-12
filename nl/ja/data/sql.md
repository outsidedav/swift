---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# データの永続性のために SQL データベースを使用する
{: #sql_data}

構造化照会言語 (SQL) は、リレーショナル・データベースでデータを管理するために使用されるドメイン固有言語です。 データの使用中にサーバーがシャットダウンする場合に備えて、データを永続化することをお勧めします。データに永続性を付与するには、SQL データベースを Swift から直接使用します。
Swift の最も重要な特長の 1 つに、型の安全性があります。 Swift と共に SQL データベースを使用することは論理的な選択です。両方で、型の安全性がサポートされるためです。

## SQL データベースでの ORM の使用

オブジェクト関連マッピング (ORM) を使用すると、SQL ステートメントを扱うことなく、オブジェクトをリレーショナル・データベースにマップできます。 すると、構文解析と逐次化の多くを自分で行うことなく、リレーショナル・データベースでオブジェクトを保管したり取得したりできるようになります。

## ステップ 1. ORM の使用を開始する

[PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) や [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL) などの SQL プラグインで、[Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM) を使用します。

この例では、[PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) プラグインを使用します。 このプラグインをインストールするには、[ここ](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation)に示された手順に従ってください。

## ステップ 2. ORM をアプリケーションにインポートする

1. `Swift-Kuery-ORM` および `Swift-Kuery-PostgreSQL` パッケージを追加して、`Package.swift` ファイルを更新します。
  ```swift
     dependencies: [
      ...
      // Add these two lines
      .package(url: "https://github.com/IBM-Swift/Swift-Kuery-ORM.git", from: "0.0.1"),
      .package(url: "https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL.git", from: "1.0.0"),
    ],
    targets: [
      .target(
        name: ...
        // Add these two modules to your target(s) dependencies
        dependencies: [..., "SwiftKueryORM", "SwiftKueryPostgreSQL"]),
    ]
  ```
  {: codeblock}

2. ORM 機能を追加するには、`Application.swift` の先頭に、次の `import` 行を追加します。
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## ステップ 3. データベースを作成する

1. マシン上で PostgreSQL をセットアップしたら、ターミナルを使用してデータベースを作成します。
  ```bash
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. `Application.swift` でデータベースを初期設定します。
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **注**: 並行要求のために接続プールを使用します。

## ステップ 4. モデルを定義する

1. モデル `Grade` を作成します。
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. その後、表を作成します。
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    // Error
  }
  ```
  {: pre}

## ステップ 5. データを管理する

### データの保存

`Grade` のインスタンスを保存するには、インスタンスを作成し、`save()` を呼び出します。
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### データの検索

データベースからすべての Grade を取得するには、静的呼び出し `findAll()` を使用します。
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### データの更新

同様の方法で、データベース内の Grade を更新します。
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### データの削除

同様の方法で、データベースから Grade を削除します。
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

これらのすべての呼び出しでは、1 回呼び出されるハンドラーを取得し、データベース呼び出しの完了時に実行します。

## Kitura での ORM の使用

ORM を簡単に試用できるように、[FoodTrackerBackend チュートリアル](https://github.com/IBM/FoodTrackerBackend)では、iOS アプリから PostgreSQL データベースに直接、Meal オブジェクトを保存およびフェッチすることができます。チュートリアルを完了しても、再度実行して、Swift-Kuery-ORM の利点や、Kitura コードをどのように簡略化できるのかを確認するのは有益です。

## Swift-Kuery を直接使用

データベースをより詳細に制御する必要があり、ORM では制約が多い場合は、SQL 抽象化層 [Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery) を使用して、そこで SQL 照会を作成することができます。
