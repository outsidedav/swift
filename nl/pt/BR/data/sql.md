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

# Usando um banco de dados SQL para persistência de dados
{: #sql_data}

A Linguagem de Consulta Estruturada (SQL) é uma linguagem específica do domínio usada para gerenciar dados em bancos de dados relacionais. A persistência de dados é recomendada para quando o servidor é encerrado enquanto você está utilizando-o. Para incluir a persistência de dados, é possível usar um Banco de dados SQL diretamente do Swift. 

Um dos recursos mais importantes do Swift é a sua segurança de tipo. Usar um banco de dados SQL com o Swift é uma escolha lógica, porque a segurança de tipo é suportada por ambos.

## Usando o ORM com um Banco de Dados SQL
{: sql-orm}

Com o Mapeamento de objeto relacional (ORM), é possível mapear objetos para bancos de dados relacionais sem precisar lidar com instruções SQL. Em seguida, será possível armazenar e recuperar objetos de um banco de dados relacional sem executar muito da análise e da própria serialização.

## Etapa 1. Introdução ao ORM
{: #start-orm}

Use o [Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") com um plug-in da SQL como [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ou [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

Para este exemplo, o plug-in do [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") é usado. Siga as instruções para [instalar o plug-in](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Etapa 2. Importando o ORM para seu aplicativo
{: #import-orm}

1. Atualize o arquivo `Package.swift` incluindo os pacotes `Swift-Kuery-ORM` e `Swift-Kuery-PostgreSQL`:
  ```swift
     dependencies: [
      ...
      / * Inclua estas duas linhas */
      .package (url: "https: //github.com/IBM-Swift/Swift-Kuery-ORM.git ", de:" 0.0.1 "),
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

2. Para incluir a funcionalidade ORM, inclua as seguintes linhas `import` no início de seu `Application.swift`:
  ```swift
  import SwiftKueryORM import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## Etapa 3. Criando seu Banco de Dados
{: #create-db-sql}

1. Depois que PostgreSQL for configurado em sua máquina, use um terminal para criar o banco de dados:
  ```
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. Inicialize o banco de dados em seu `Application.swift`:
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **Nota**: um conjunto de conexões é usado para solicitações simultâneas.

## Etapa 4. Definindo seu modelo
{: #define-model-sql}

1. Crie o modelo,  ` Grade `:
  ```swift
  struct Grade: Modelo {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. Em seguida, crie sua tabela:
  ```swift
  fazer {
    try Grade.createTableSync()
  } catch {
    / * Erro */ }
  ```
  {: pre}

## Etapa 5. Gerenciando seus dados
{: #manage-data-sql}

### Salvando Dados
{: #save-data-sql}

Para salvar uma instância de `Grade`, crie uma instância e chame `save()`:
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### Localizando Dados
{: #find-data-sql}

Para recuperar todas as Notas do banco de dados, é possível usar a chamada estática `findAll()`:
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Atualizando Dados
{: update-data-sql}

Uma abordagem semelhante é usada para atualizar uma Nota do banco de dados:
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### Excluindo dados
{: #delete-data-sql}

Uma abordagem semelhante é usada para excluir uma Nota do banco de dados.
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

Todas essas chamadas usam um manipulador que é chamado uma vez e o executa quando a chamada do banco de dados é concluída.

## Usando o ORM com Kitura
{: #kitura-orm}

Para facilitar a experimentação do ORM, o [tutorial do FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") pode salvar e buscar objetos Meal por meio do app do iOS diretamente em um banco de dados do PostgreSQL. Mesmo se você concluir o tutorial, vale a pena consultá-lo novamente para ver o poder do Swift-Kuery-ORM e como ele pode simplificar o seu código do Kitura.

## Usando Swift-Kuery diretamente
{: #swift-kuery}

Se o ORM limitar você porque é necessário mais controle sobre o seu banco de dados, será possível usar a camada de abstração de SQL, [Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), na qual será possível fazer uma consulta SQL.
