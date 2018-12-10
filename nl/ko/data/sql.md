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

# 데이터 지속성을 위한 SQL 데이터베이스 사용
{: #sql_data}

SQL(Structured Query Language)은 관계형 데이터베이스에서 데이터 관리를 위해 사용되는 도메인별 언어입니다. 서버를 사용 중인 동안에 서버가 종료되는 경우 데이터 지속성이 권장됩니다. 데이터 지속성을 추가하기 위해 Swift에서 직접 SQL 데이터베이스를 사용할 수 있습니다.
Swift의 가장 중요한 기능 중 하나는 유형 안전성(type-safety)입니다. 두 가지 모두에서 유형 안전성이 지원되므로 Swift에서 SQL 데이터베이스를 사용하는 것은 적절한 선택입니다.

## SQL 데이터베이스에서 ORM 사용

ORM(Object-Relational Mapping)을 사용하면 SQL문을 처리하지 않고도 오브젝트를 관계형 데이터베이스로 맵핑할 수 있습니다. 그런 다음 많은 양의 구문 분석 및 직렬화를 자체적으로 수행하지 않고도 관계형 데이터베이스에서 오브젝트를 저장하고 검색할 수 있습니다.

## 1단계. ORM 시작하기

SQL 플러그인(예: [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) 또는 [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL))으로 [Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM)을 사용하십시오.

이 예제에서는 [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) 플러그인이 사용됩니다. 플러그인을 설치하려면 지시사항([여기](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation))을 따르십시오.

## 2단계. 애플리케이션에 ORM 가져오기

1. `Swift-Kuery-ORM` 및 `Swift-Kuery-PostgreSQL` 패키지를 추가하여 `Package.swift` 파일을 업데이트하십시오.
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

2. ORM 기능을 추가하려면 `Application.swift`의 시작 부분에 다음 `import` 행을 추가하십시오.
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## 3단계. 데이터베이스 작성

1. 머신에 PostgreSQL이 설정된 후 터미널을 사용하여 데이터베이스를 작성하십시오.
  ```bash
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. `Application.swift`에서 데이터베이스를 초기화하십시오.
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **참고**: 동시 요청을 위해 연결 풀이 사용됩니다.

## 4단계. 모델 정의

1. `Grade` 모델을 작성하십시오.
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. 그런 다음 테이블을 작성하십시오.
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    // Error
  }
  ```
  {: pre}

## 5단계. 데이터 관리

### 데이터 저장

`Grade`의 인스턴스를 저장하려면 인스턴스를 작성하고 `save()`를 호출하십시오.
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### 데이터 찾기

데이터베이스에서 모든 등급을 검색하기 위해 정적 호출 `findAll()`을 사용할 수 있습니다.
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### 데이터 업데이트

유사한 접근 방법이 데이터베이스에서 등급을 업데이트하는 데 사용됩니다.
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### 데이터 삭제

유사한 접근 방법이 데이터베이스에서 등급을 삭제하는 데 사용됩니다.
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

이러한 모든 호출은 한 번 호출되는 핸들러를 사용하며 데이터베이스 호출이 완료되면 이를 실행합니다.

## Kitura에서 ORM 사용

ORM을 좀 더 쉽게 시험 사용해 볼 수 있도록 [FoodTrackerBackend 튜토리얼](https://github.com/IBM/FoodTrackerBackend)에서는 iOS 앱에서 Meal 오브젝트를 저장하고 PostgreSQL 데이터베이스로 직접 페치할 수 있습니다. 튜토리얼을 완료한 경우에도 Swift-Kuery-ORM 기능과 Kitura 코드를 간소할 수 있는 방법을 확인하기 위해 튜토리얼을 다시 살펴보는 것이 좋습니다. 

## Swift-Kuery 직접 사용

데이터베이스에 대한 더 많은 제어가 필요하므로 ORM이 사용자를 제한하는 경우 SQL 조회를 작성할 수 있는 SQL 추상화 계층인 [Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery)를 사용할 수 있습니다.
