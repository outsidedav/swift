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

# Utilisation d'une base de données SQL pour la persistance des données
{: #sql_data}

Le langage SQL (Structured Query Language) est un langage spécifique à un domaine qui est utilisé pour la gestion des données dans les base de données relationnelles. La persistance des données est recommandé en cas d'arrêt de votre serveur en cours d'utilisation. Pour ajouter la persistance des données, vous pouvez utiliser une base de données SQL directement depuis Swift. 

L'une des fonctionnalités les plus importantes de Swift est son type de sécurité. L'utilisation d'une base de données SQL avec Swift est un choix logique car le type de sécurité est pris en charge par les deux.

## Utilisation de ORM avec une base de données SQL
{: sql-orm}

Avec ORM (Object-Relational Mapping), vous pouvez mapper des objets aux bases de données relationnelles sans avoir à traiter des instructions SQL. Vous pouvez ensuite stocker et extraire des objets d'une base de données relationnelle sans vous occuper de l'analyse syntaxique et de la sérialisation vous-même.

## Etape 1. Mise en route avec ORM
{: #start-orm}

Utilisez [Swift-Kuery-ORM](https://github.com/IBM-Swift/Swift-Kuery-ORM){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") avec un plug-in SQL tel que [PostgreSQL](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") ou [MySQL](https://github.com/IBM-Swift/SwiftKueryMySQL){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

Dans cet exemple, le plug-in [PostgreSQL](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") est utilisé. Suivez les instructions pour l'[installation du plug-in](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe").

## Etape 2. Importation de ORM dans votre application
{: #import-orm}

1. Mettez à jour le fichier `Package.swift` en ajoutant les packages `Swift-Kuery-ORM` et `Swift-Kuery-PostgreSQL` :
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

2. Pour ajouter une fonctionnalité ORM, ajoutez les lignes `import` suivantes au début de votre `Application.swift` :
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## Etape 3. Création de votre base de données
{: #create-db-sql}

1. Une fois PostgreSQL configuré sur votre machine, utilisez un terminal pour créer la base de données :
  ```
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
{: #define-model-sql}

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
    /* Error */
  }
  ```
  {: pre}

## Etape 5. Gestion de vos données
{: #manage-data-sql}

### Sauvegarde des données
{: #save-data-sql}

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
{: #find-data-sql}

Pour extraire toutes les instances Grades de la base de données, vous pouvez utiliser l'appel statique, `findAll()` :
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Mise à jour des données
{: update-data-sql}

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
{: #delete-data-sql}

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
{: #kitura-orm}

Pour simplifier le test de ORM, le [tutoriel FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe") peut sauvegarder et extraire directement des objets Meal de l'application iOS vers une base de données PostgreSQL. Même si vous terminez le tutoriel, cela vaut la peine de le repasser pour voir la puissance de Swift-Kuery-ORM, et comment il peut simplifier votre code Kitura.

## Utilisation directe de Swift-Kuery
{: #swift-kuery}

Si ORM vous limite car vous avez besoin de plus de contrôle sur votre base de données, vous pouvez utiliser la couche d'abstraction SQL, [Swift-Kuery](https://github.com/IBM-Swift/Swift-Kuery){: new_window} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"), dans laquelle vous pouvez créer une requête SQL.
