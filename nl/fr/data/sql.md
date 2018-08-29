---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilisation d'une base de données SQL pour la persistance des données
{: #sql_data}

La langage SQL (Structured Query Language) est un langage spécifique à un domaine qui est utilisé pour la gestion des données dans les base de données relationnelles. Il est recommandé de conserver vos données dans le cas où votre serveur s'arrête en cours d'utilisation. Pour ajouter la persistance des données, vous pouvez utiliser une base de données SQL directement depuis Swift. L'une des fonctionnalités les plus importantes de Swift est son type de sécurité. L'utilisation d'une base de données SQL avec Swift est un choix logique car le type de sécurité est pris en charge par les deux.

## Utilisation de ORM avec une base de données SQL

Avec ORM (Object-Relational Mapping), vous pouvez mapper des objets aux bases de données relationnelles sans avoir à traiter des instructions SQL. Vous pouvez ensuite stocker et extraire des objets d'une base de données relationnelle sans vous occuper de l'analyse syntaxique et de la sérialisation vous-même.

## Step 1. Mise en route avec ORM

Utilisez [Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM) avec un plug-in SQL tel que [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) ou [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL).

Dans cet exemple, le plug-in [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) est utilisé. Suivez les instructions pour l'installation du plug-in [ici](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation).

## Etape 2. Importation de ORM dans votre application

1. Mettez à jour le fichier `Package.swift` en ajoutant les packages `Swift-Kuery-ORM` et `Swift-Kuery-PostgreSQL` :
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

2. Pour ajouter une fonctionnalité ORM, ajoutez les lignes `import` suivantes au début de votre `Application.swift` :
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## Etape 3. Création de votre base de données

1. Une fois PostgreSQL configuré sur votre machine, utilisez un terminal pour créer la base de données :
  ```bash
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. Initialisez la base de données dans votre `Application.swift` :
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **Remarque **: Un pool de connexions est utilisé pour les demandes simultanées.

## Etape 4. Définition de votre modèle

1. Créez le modèle, `Grade`:
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. Ensuite, créez votre table :
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    // Error
  }
  ```
  {: pre}

## Etape 5. Gestion de vos données

### Sauvegarde des données

Pour sauvegarder une instance de `Grade`, créez une instance, puis appelez `save()` :
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### Recherche de données

Pour extraire toutes les instances Grades de la base de données, vous pouvez utiliser l'appel statique, `findAll()` :
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Mise à jour des données

Une approche similaire est utilisée pour mettre à jour une instance Grade de la base de données :
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### Suppression de données

Une approche similaire est utilisée pour supprimer une instance Grade de la base de données.
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

Tous ces appels prennent un gestionnaire qui est appelé une fois et s'exécute lorsque l'appel de base de données est terminé.

## Utilisation de ORM avec Kitura

Pour simplifier le test de ORM, le [tutoriel FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) peut sauvegarder et extraire les objets Meal de l'appli iOS, ainsi que dans une base de données PostgreSQL à l'aide de ORM. Même si vous suivez le tutoriel, il est utile de l'effectuez à nouveau pour vérifier la puissance de Swift-Kuery-ORM, et comment cela peut simplifier votre code de Kitura.

## Utilisation de Swift-Kuery directement

Si ORM vous limite car vous avez besoin de plus de contrôle sur votre base de données, vous pouvez utiliser la couche d'abstraction SQL, [Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery), dans laquelle vous pouvez créer une requête SQL.
