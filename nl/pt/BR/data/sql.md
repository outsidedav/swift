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

# Usando um banco de dados SQL para persistência de dados
{: #sql_data}

A Linguagem de Consulta Estruturada (SQL) é uma linguagem específica do domínio usada para gerenciar dados em bancos de dados relacionais. Recomenda-se persistir seus dados no caso de o servidor ser encerrado enquanto está sendo usado. Para incluir a persistência de dados, é possível usar um Banco de dados SQL diretamente do Swift.
Um dos recursos mais importantes do Swift é a sua segurança de tipo. Usar um banco de dados SQL com o Swift é uma escolha lógica, porque a segurança de tipo é suportada por ambos.

## Usando o ORM com um Banco de Dados SQL

Com o Mapeamento de objeto relacional (ORM), é possível mapear objetos para bancos de dados relacionais sem precisar lidar com instruções SQL. Em seguida, será possível armazenar e recuperar objetos de um banco de dados relacional sem executar muito da análise e da própria serialização.

## Etapa 1. Introdução ao ORM

Use o [Swift-Kuery-ORM](http://github.com/IBM-Swift/Swift-Kuery-ORM) com um plug-in SQL, como [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) ou [MySQL](http://github.com/IBM-Swift/SwiftKueryMySQL).

Para este exemplo, o plug-in [PostgreSQL](http://github.com/IBM-Swift/Swift-Kuery-PostgreSQL) é usado. Siga as instruções para instalar o plug-in [aqui](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation).

## Etapa 2. Importando o ORM para seu aplicativo

1. Atualize o arquivo `Package.swift` incluindo os pacotes `Swift-Kuery-ORM` e `Swift-Kuery-PostgreSQL`:
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

2. Para incluir a funcionalidade ORM, inclua as seguintes linhas `import` no início de seu `Application.swift`:
  ```swift
  import SwiftKueryORM import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## Etapa 3. Criando seu Banco de Dados

1. Depois que PostgreSQL for configurado em sua máquina, use um terminal para criar o banco de dados:
  ```bash
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

## Etapa 4. Definindo seu Modelo

1. Crie o Modelo,  ` Classificação `:
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
    // Erro }
  ```
  {: pre}

## Etapa 5. Gerenciando seus dados

### Salvando Dados

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

Para recuperar todas as Notas do banco de dados, é possível usar a chamada estática `findAll()`:
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Atualizando Dados

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

Para facilitar a tentativa de ORM, o [tutorial do FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend) pode salvar e buscar os objetos Meal do app iOS e em um banco de dados PostgreSQL usando o ORM. Mesmo se você concluir o tutorial, vale a pena passar por ele novamente para ver o poder do Swift-Kuery-ORM e como ele pode simplificar o código Kitura.

## Usando Swift-Kuery diretamente

Se o ORM o limitar porque você precisa de mais controle sobre seu banco de dados, será possível usar a camada de abstração de SQL, [Swift-Kuery](http://github.com/IBM-Swift/Swift-Kuery), na qual será possível fazer uma consulta SQL.
