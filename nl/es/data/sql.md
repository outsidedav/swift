---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

keywords: sql swift, database swift, persistence swift, data swift, orm swift, kuery swift, kitura swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilización de una base de datos SQL para la persistencia de datos
{: #sql_data}

SQL (Structured Query Language) es un lenguaje específico del dominio que se utiliza para gestionar datos en bases de datos relacionales. Se recomienda utilizar persistencia de datos para cuando el servidor se apague mientras lo esté utilizando. Para añadir persistencia de datos, puede utilizar una base de datos SQL directamente desde Swift. 

Una de las características más importantes de Swift es su seguridad de tipo. La utilización de una base de datos SQL con Swift es una opción lógica, ya que la seguridad de tipo está soportada por ambos.

## Utilización de ORM con una base de datos SQL
{: sql-orm}

Con ORM (Object-Relational Mapping), puede correlacionar objetos con bases de datos relacionales sin tener que tratar con sentencias de SQL. A continuación, puede almacenar y recuperar objetos de una base de datos relacional sin realizar gran parte del análisis ni de la serialización usted mismo.

## Paso 1. Iniciación a ORM
{: #start-orm}

Utilice [Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") con un plugin SQL como
[PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") o
[MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

Para este ejemplo, se utiliza el plugin de [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"). Siga las instrucciones para
[instalar el plugin](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

## Paso 2. Importación de ORM en la aplicación
{: #import-orm}

1. Actualice el archivo `Package.swift` añadiendo los paquetes `Swift-Kuery-ORM` y `Swift-Kuery-PostgreSQL`:
  ```swift
     dependencies: [
      ...
      /* Añadir estas dos líneas */
      .package(url: "https://github.com/IBM-Swift/Swift-Kuery-ORM.git", from: "0.0.1"),
      .package(url: "https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL.git", from: "1.0.0"),
    ],
    targets: [
      .target(
        name: ...
        /* Añadir estos dos módulos a sus dependencias de destino */
        dependencies: [..., "SwiftKueryORM", "SwiftKueryPostgreSQL"]),
    ]
  ```
  {: codeblock}

2. Para añadir la funcionalidad ORM, añada las siguientes líneas de `importación` al principio de su `Application.swift`:
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## Paso 3. Creación de la base de datos
{: #create-db-sql}

1. Una vez que PostgreSQL se haya configurado en la máquina, utilice un terminal para crear la base de datos:
  ```
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. Inicialice la base de datos en el archivo `Application.swift`:
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **Nota**: Se utiliza una agrupación de conexiones para las solicitudes simultáneas.

## Paso 4. Definición del modelo
{: #define-model-sql}

1. Cree el modelo, `Grade`:
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. A continuación, cree la tabla:
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    /* Error */
  }
  ```
  {: pre}

## Paso 5. Gestión de los datos
{: #manage-data-sql}

### Guardado de datos
{: #save-data-sql}

Para guardar una instancia de `Grade` (Nota), cree una instancia y llame a `save()`:
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Guardar en la base de datos
grade.save { student, error in
  ...
}
```
{: pre}

### Búsqueda de datos
{: #find-data-sql}

Para recuperar todas las notas de la base de datos, puede utilizar la llamada estática, `findAll()`:
```swift
// Obtener una matriz de notas en la base de datos
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Actualización de datos
{: update-data-sql}

Se utiliza un enfoque similar para actualizar una nota desde la base de datos:
```swift
grade.grade = 80

// Actualizar la nota
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### Supresión de datos
{: #delete-data-sql}

Se utiliza un enfoque similar para suprimir una nota de la base de datos.
```swift
// Suprimir la nota
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

Todas estas llamadas toman un manejador que se llama una vez y que lo ejecuta cuando se completa la llamada a base de datos.

## Utilización de ORM con Kitura
{: #kitura-orm}

Para facilitar la prueba de ORM, la [guía de aprendizaje de FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") puede guardar y captar objetos Meal de la app de iOS directamente en una base de datos PostgreSQL. Aunque complete la guía de aprendizaje, vale la pena repasarla de nuevo para ver la potencia de Swift-Kuery-ORM y cómo puede simplificar su código Kitura.

## Utilización de Swift-Kuery directamente
{: #swift-kuery}

Si ORM le limita porque necesita un mayor control sobre la base de datos, puede utilizar la capa de abstracción de SQL, [Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"), donde puede realizar una consulta SQL.
