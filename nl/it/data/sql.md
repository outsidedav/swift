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

# Utilizzo di un database SQL per la persistenza dei dati
{: #sql_data}

SQL (Structured Query Language) è un linguaggio specifico per il dominio che viene utilizzato per gestire i dati nei database relazionali. La persistenza dei dati è consigliata quando il tuo server si arresta mentre lo stai utilizzando. Per aggiungere la persistenza dei dati, puoi utilizzare un database SQL direttamente da Swift. 

Una delle funzioni più importanti da Swift è la sua indipendenza dai tipi. L'utilizzo di un database SQL con Swift è una scelta logica in quanto l'indipendenza dai tipi è supportata da entrambi.

## Utilizzo di ORM con un database SQL
{: sql-orm}

Con ORM (Object-Relational Mapping), puoi associare gli oggetti ai database relazionali senza dover avere a che fare con le istruzioni SQL. Puoi quindi archiviare e richiamare gli oggetti da un database relazionale senza dover eseguire tu stesso la maggior parte dell'analisi e della serializzazione.

## Passo 1. Introduzione a ORM
{: #start-orm}

Utilizza [Swift-Kuery-ORM](https://github.com/IBM-Swift/Swift-Kuery-ORM){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") con un plugin SQL come [PostgreSQL](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") o [MySQL](https://github.com/IBM-Swift/SwiftKueryMySQL){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

Per questo esempio, viene utilizzato il plug-in [PostgreSQL](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"). Attieniti alle istruzioni per [installare il plug-in](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno").

## Passo 2. Importazione di ORM nella tua applicazione
{: #import-orm}

1. Aggiorna il file `Package.swift` aggiungendo i pacchetti `Swift-Kuery-ORM` e `Swift-Kuery-PostgreSQL`;
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

2. Per aggiungere la funzionalità ORM, aggiungi le seguenti righe `import` all'inizio del tuo `Application.swift`:
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## Passo 3. Creazione del tuo database
{: #create-db-sql}

1. Dopo che PostgreSQL è stato configurato sulla tua macchina, utilizza un terminale per creare il database:
  ```
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. Inizializza il database nel tuo `Application.swift`:
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **Nota**:per le richieste simultanee viene utilizzato un pool di connessioni.

## Passo 4. Definizione del tuo modello
{: #define-model-sql}

1. Crea il modello, `Grade`:
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. Crea quindi la tua tabella:
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    /* Error */
  }
  ```
  {: pre}

## Passo 5. Gestione dei tuoi dati
{: #manage-data-sql}

### Salvataggio dei dati
{: #save-data-sql}

Per salvare un'istanza di `Grade`, crea un'istanza e richiama `save()`:
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### Individuazione dei dati
{: #find-data-sql}

Per richiamare tutti i Grade dal database, puoi utilizzare la chiamata statica `findAll()`:
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Aggiornamento dei dati
{: update-data-sql}

Un approccio simile viene utilizzato per aggiornare un Grade dal database:
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### Eliminazione dei dati
{: #delete-data-sql}

Un approccio simile viene utilizzato per eliminare un Grade dal database.
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

Tutte queste chiamate impiegano un gestore che viene richiamato una sola volta e viene eseguito quando la chiamata al database è completa.

## Utilizzo di ORM con Kitura
{: #kitura-orm}

Per facilitare la prova di ORM, l'[esercitazione FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") può salvare e recuperare gli oggetti Meal dall'applicazione iOS direttamente in un database PostgreSQL. Anche se completi l'esercitazione, vale la pena ripercorrerla nuovamente per vedere la potenza di Swift-Kuery-ORM e come possa semplificare il tuo codice Kitura.

## Utilizzo di Swift-Kuery direttamente
{: #swift-kuery}

Se ORM ti limita perché hai bisogno di più controllo sul tuo database, puoi utilizzare il livello di astrazione SQL [Swift-Kuery](https://github.com/IBM-Swift/Swift-Kuery){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno"), dove puoi effettuare una query SQL.
