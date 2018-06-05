---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# SQL
{: #sql_data}

Structured Query Language (SQL) is a domain-specific language that is used for managing data in relational databases.

## Using an SQL Database for data persistence

It is recommended to persist your data in case your server shuts down while you're using it. To add data persistence, you can use an SQL Database directly from Swift.

One of the most important features from Swift is its type-safety. Using an SQL database with Swift is a logical choice because type-safety is supported by both.

## Using an ORM with an SQL Database

With Object-Relational Mapping (ORM), you can map objects to relational databases without having to deal with SQL statements. You can then store and retrieve objects from a relational database without doing much of the parsing and serialization yourself.

## Getting started with the ORM

Use the [Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM) with an SQL plug-in such as [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) or [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL).

For this example, the [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) plug-in is used. Follow the instructions to install the plug-in [here](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation).

### Importing the ORM into your application

1. Update `Package.swift` by adding the Swift-Kuery-ORM and Swift-Kuery-PostgreSQL packages:
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

2. To add ORM functionality, add the following `import` lines at the beginning of your `Application.swift`:
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

### Creating your database

1. After PostgreSQL is set up on your machine, use a terminal to create the database:
  ```bash
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. Initialize the database in your `Application.swift`:
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

**Note**: A connection pool is used for concurrent requests.

### Defining your Model

1. Create the Model, `Grade`:
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. Then, create your table:
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    // Error
  }
  ```
  {: pre}

## Persisting your data

### Saving data

To save an instance of `Grade`, create an instance, and call `save()`:
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### Finding data

To retrieve all the Grades from the database, you can use the static call, `findAll()` :
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Updating data

A similar approach is used to update a Grade from the database:
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### Deleting data

A similar approach is used to delete a Grade from the database.
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

All of these calls take a handler that is called once and runs it when the database call is complete.

## Using the ORM with Kitura

To make it easier to try out the ORM, the [FoodTrackerBackend tutorial](https://github.com/IBM/FoodTrackerBackend) can save and fetch the Meal objects from the iOS app, and into a PostgreSQL database by using the ORM. Even if you complete the tutorial, it is worth going through it again to see the power of Swift-Kuery-ORM, and how it can simplify your Kitura code.

## Using Swift-Kuery directly

If the ORM limits you because you need more control over your database, you can use the SQL abstraction layer, [Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery), where you can make an SQL query.
