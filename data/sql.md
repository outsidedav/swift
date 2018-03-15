---

copyright:
  years: 2017
lastupdated: "2017-12-19"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# SQL
{: #sql_data}
Structured Query Language (SQL) is a domain-specific language used for managing data in relational databases.

## Persist Data with a SQL Database

It is strongly recommended to persist your data just in case your server shuts down while you're using it. To add this data persistence, we can use a SQL Database directly from
Swift.

One the most important from Swift is its type safety. Using a SQL database with Swift is a logical choice because type-safety is supported by both.

## Using an ORM with a SQL Database

An Object-Relational Mapping (ORM) enables you to map objects to relational databases without having to deal with SQL statements. You can then store and retrieve objects from a relational database without doing a lot of parsing and serialization yourself.

## Getting started with the ORM
Use the [Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM) with a SQL plugin such as [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) or [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL).

For this example, we will be using the [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) plugin. Please follow the instructions to install [here](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation).

### Importing the ORM in your application

Go to your `Package.swift` and add Swift-Kuery-ORM and Swift-Kuery-PostgreSQL:

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
{: pre}

To add ORM functionality to your `Application.swift`, you will need to import Swift-Kuery-ORM and Swift-Kuery-PostgreSQL at the top of the file:

```swift
import SwiftKueryORM
import SwiftKueryPostgreSQL
```
{: pre}

### Create Your Database

After setting up PostgreSQL on your machine, go to your terminal and create a database:

```bash
brew services start postgresql
createdb school
```
{: pre}
Initialize your database in your `Application.swift`:

```swift
let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
Database.default = Database(pool)
```
{: pre}

**Note** We use a connection pool when we have concurrent requests.

### Define your Model

Let's create the Model, Grade:

```swift
struct Grade: Model {
  var course: String
  var grade: Int
}
```
{: pre}

Then, create your table:

```swift
do {
  try Grade.createTableSync()
} catch {
  // Error
}
```
{: pre}

## Persisting your data

### Saving

To save an instance of `Grade`, we create an instance and call
`save()`:

```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### Finding

To retrieve all the Grades from the database, you can use the static call, `findAll()` :

```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Updating

A similar approach is used to update a Grade from the database:

```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### Deleting

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

In order to make it easier to get hands on and try out the ORM, our [FoodTrackerBackend tutorial](https://github.com/IBM/FoodTrackerBackend) has been updated to save and fetch the Meal objects from the iOS app to a PostgreSQL database using the ORM. Even if you’ve done the tutorial before, it’s well worth going through it again to see the power of Swift-Kuery-ORM and how it can simplify your Kitura code.

## Using Swift-Kuery directly

If the ORM limits you and you need more control over you database, you can use the SQL abstraction layer, [Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery), where you can make an SQL query.
