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

# SQL-Datenbank für Datenpersistenz nutzen
{: #sql_data}

Structured Query Language (SQL) ist eine fachspezifische Sprache, die zur
Verwaltung von Daten in relationalen Datenbanken verwendet wird. Für den Fall,
dass Ihr Server beendet wird, während Sie ihn gerade nutzen, empfiehlt es sich,
Daten persistent zu speichern. Zur Gewährleistung der Datenpersistenz können
Sie direkt aus Swift heraus eine SQL-Datenbank verwenden.
Eines der wichtigsten
Merkmale von Swift ist seine Typsicherheit. Die Verwendung einer SQL-Datenbank
mit Swift ist eine logische Option, da die Typsicherheit von beiden unterstützt
wird.

## ORM mit einer SQL-Datenbank verwenden
{: sql-orm}

Mittels ORM (Object-Relational Mapping, objektrelationale Zuordnung)
können Sie Objekte zu relationalen Datenbanken zuordnen, ohne sich mit
SQL-Anweisungen auseinandersetzen zu müssen. Danach können Sie Objekte in einer
relationalen Datenbank speichern und daraus abrufen, ohne sich selbst um
Syntaxanalyse und Serialisierung kümmern zu müssen.

## Schritt 1. Einführung in ORM
{: #start-orm}

Verwenden Sie
[Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM)
mit einem SQL-Plug-in wie beispielsweise
[PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL)
oder [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL).

In diesem Beispiel wird das Plug-in [ PostgreSQL
](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) verwendet. Befolgen Sie die
[hier](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation)
verfügbaren Anweisungen, um das Plug-in zu installieren.

## Schritt 2. ORM in Ihre Anwendung importieren
{: #import-orm}

1. Aktualisieren Sie die Datei `Package.swift`,
indem Sie die Pakete `Swift-Kuery-ORM` und `Swift-Kuery-PostgreSQL` hinzufügen:
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

2. Fügen Sie die folgenden Zeilen für `import` am
Anfang Ihrer Datei `Application.swift` hinzu, um die ORM-Funktionalität hinzuzufügen:
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## Schritt 3. Datenbank erstellen
{: #create-db-sql}

1. Nachdem Sie PostgreSQL auf Ihrer Maschine konfiguriert haben,
verwenden Sie ein Terminal, um die Datenbank zu erstellen:
  ```
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. Initialisieren Sie die Datenbank in
Ihrer Datei `Application.swift`:
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **Hinweis**: Für gleichzeitige Anforderungen wird
ein Verbindungspool genutzt.

## Schritt 4. Modell definieren
{: #define-model-sql}

1. Erstellen Sie das Modell `Grade`:
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. Erstellen Sie anschließend Ihre Tabelle:
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    /* Error */
  }
  ```
  {: pre}

## Schritt 5. Daten verwalten
{: #manage-data-sql}

### Daten speichern
{: #save-data-sql}

Um eine Instanz des Modells `Grade` zu speichern,
erstellen Sie eine Instanz und rufen Sie die Methode `save()`
auf:
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### Daten suchen
{: #find-data-sql}

Um alle Grade-Instanzen aus der Datenbank abzurufen, können Sie den
statischen Aufruf `findAll()` verwenden:
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Daten aktualisieren
{: update-data-sql}

Eine ähnliche Methode wird verwendet, um eine Grade-Instanz aus der
Datenbank zu aktualisieren:
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### Daten löschen
{: #delete-data-sql}

Eine ähnliche Methode wird verwendet, um eine Grade-Instanz aus der
Datenbank zu löschen.
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

Alle diese Aufrufe verwenden einen Handler, der einmal aufgerufen und
nach Abschluss des Datenbankaufrufs ausgeführt wird.

## ORM mit Kitura verwenden
{: #kitura-orm}

Um das Ausprobieren von ORM zu vereinfachen, kann das Lernprogramm
[FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend)
die Objekte "Meal" aus der iOS-App direkt in eine PostgreSQL-Datenbank abrufen. Auch wenn Sie das Lernprogramm bereits abgeschlossen haben,
ist es sinnvoll, es noch einmal durchzuarbeiten, denn es demonstriert die
Leistungsstärke von Swift-Kuery-ORM und zeigt, wie Ihr Kitura-Code hiermit
vereinfacht werden kann.

## Swift-Kuery direkt verwenden
{: #swift-kuery}

Wenn Sie durch ORM eingeschränkt werden, weil Sie eine größere Steuerung
für Ihre Datenbank benötigen, können Sie die SQL-Abstraktionsebene
[Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery)
verwenden, auf der Sie eine SQL-Abfrage ausgeben können.
