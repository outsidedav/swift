---

copyright:
  years: 2017-2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Armazenando documentos no  {{site.data.keyword.cloud_notm}}
{: #cloudant}

O {{site.data.keyword.cloudantfull}} é um Banco de dados como um serviço (DBaaS) orientado a documentos. Ele armazena dados como documentos no formato JSON. Ele é construído com escalabilidade, alta disponibilidade e durabilidade em mente, além de ser fácil de configurar para uso em aplicativos Swift. Ele é fornecido com uma ampla variedade de opções de indexação que incluem MapReduce,
Consulta do {{site.data.keyword.cloudant_short_notm}},
índice de texto total
e indexação geoespacial. Os recursos de replicação facilitam a manutenção de dados em sincronia entre os
clusters de banco de dados, PCs desktop e dispositivos móveis. 
{:shortdesc}

Para obter todas as maneiras de uso do {{site.data.keyword.cloudant_short_notm}}, consulte [Características básicas do {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/basics/index.html#cloudant-nosql-db-basics).

## Antes de começar

Primeiro, verifique se os pré-requisitos a seguir estão prontos para execução:
 * CocoaPods (versão 1.1.0 ou superior)
 * iOS (versão 9 ou superior)
 * MacOS (versão 10.11.5 ou superior)
 * Xcode (versão 9.0.1 ou superior)

O [{{site.data.keyword.cloudant_short_notm}} SDK for Swift![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudant/swift-cloudant) é construído com o Swift 3.2. Se estiver planejando usar o {{site.data.keyword.cloudant_short_notm}} com o Kitura, efetue check-out da [Biblioteca Kitura-CouchDB ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM-Swift/Kitura-CouchDB), que é construída com o Swift 4.0.
{: tip}

## Etapa 1. Criando uma instância do  {{site.data.keyword.cloudant_short_notm}}

Consulte [Criando uma instância do Cloudant NoSQL DB no tutorial do IBM Cloud ![Ícone de link externo](../images/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/services/Cloudant/tutorials/create_service.html#creating-a-cloudant-nosql-db-instance-on-ibm-cloud){:new_window} para provisionar uma instância do serviço.


## Etapa 2. Instalando o SDK

### Instalando o iOS Swift SDK

Use o Swift Cloudant SDK para ajudar a tornar a codificação do app mais fácil. O SDK deve ser instalado em seu código de app.

1. Abra o diretório de projeto do Xcode existente para o `Podfile`.
2. No destino de seus projetos, inclua uma dependência para o pod `SwiftCloudant`. Certifique-se
de que o `use_frameworks!` também está sob o seu destino, conforme mostrado no exemplo a seguir.
    ```
    target '<yourTarget>' do
      use_frameworks!
        pod 'SwiftCloudant', :git => 'https://github.com/cloudant/swift-cloudant.git'
    end
    ```
    {: screen}
3. Faça download da dependência  ` SwiftCloudant ` .
    ```
    instalação do pod
    ```
    {: pre}

### Instalando o SDK do Swift do Lado do Servidor

Para usar com o Swift Package Manager para desenvolvimento do lado do servidor, inclua a linha a seguir em suas dependências no `Package.swift`:
```swift
.Package (url: "https: //github.com/cloudant/swift-cloudant.git")
```
{: pre}

## Etapa 3. Inicializando o SDK

Depois de inicializar o SDK em seu app, é possível começar a alavancar o {{site.data.keyword.cloudant_short_notm}} para armazenar dados.

1.  Inclua a importação a seguir em seu arquivo swift `AppDelegate.swift` ou do lado do servidor.
    ```
    import SwiftCloudant
    ```
    {:pre}
2. Inicialize a conexão com o banco de dados.
    ```swift
    let cloudantURL = NSURL(string: "https://username.cloudant.com")!
    let client = CouchDBClient(url: cloudantURL, username: "username", password: "password")
    let dbName = "database"
    ```
    {: codeblock}

### Operações Básicas
Essas operações básicas ilustram as ações fundamentais para criar, ler e destruir seus documentos usando o cliente inicializado.

#### Criar um documento
```swift
let create = PutDocumentOperation(id: "doc1", body: ["hello": "world"], databaseName: dbName) {(response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while creating a document. Erro: \(error)") } else {
        print("Created document \(response?["id "]) com ID de revisão \ (response? ["rev "])")
    }
} client.add (operação: criar)
```
{: codeblock}

#### Ler um documento
```swift
let read = GetDocumentOperation(id: "doc1", databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while reading a document. Erro: \(error)") } else {
        print ("Documento de leitura: \ (resposta)")
    }   
}
client.add(operation: read)
```
{: codeblock}

#### Excluir um documento
```swift
let delete = DeleteDocumentOperation(id: "doc1",
    revision: "1-revisionidhere",
    databaseName: dbName) { (response, httpInfo, error) in
    if let error = error {
        print("Encountered an error while deleting a document. Erro: \(error)") } else {
        print ("Documento excluído")
    }   
}
client.add(operation: delete)
```
    {: codeblock}


## Etapa 4. Testando o app
{: #cloudant_testing}

Está tudo acertado corretamente? Teste-o para fora!

1. Execute o aplicativo, certificando-se de chamar a inicialização e as respectivas operações, como a criação de um documento.
2. Retorne para a instância de serviço do {{site.data.keyword.cloudant_short_notm}} criada anteriormente em seu navegador da web e abra o painel de serviço.
3. Selecione o banco de dados usado e será possível ver os documentos no painel.

Tendo problemas? Efetue check-out da  [ {{site.data.keyword.cloudant_short_notm}}  Referência de API ](/docs/services/Cloudant/api/index.html#api-reference-overview).


## Próximas etapas
{: #cloudant_next notoc}

Ótimo trabalho! Você incluiu um nível de persistência segura em seu app. Tente uma das opções a seguir para manter o ritmo:

* Visualize o código-fonte do [{{site.data.keyword.cloudant_short_notm}} SDK for Swift![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudant/swift-cloudant).
* Os Starter Kits são uma das maneiras mais rápidas de alavancar os recursos do IBM Cloud. O kit do iniciador **Infinite Scrolling with Cloudant NoSQL for iOS** ilustra como ampliar um ViewController para exibir dados usando paginação. Esse padrão de app é comum para desenvolvedores do iOS, além de ser um grande exemplo para ilustrar os recursos do {{site.data.keyword.cloudant_short_notm}}. Visualize os kits do iniciador disponíveis no [Painel do desenvolvedor do dispositivo móvel](https://console.bluemix.net/developer/mobile/dashboard). Faça download do código. Execute o app.
* Obtenha mais informações e aproveite todos os recursos que o {{site.data.keyword.cloudant_short_notm}} tem para oferecer; [verifique os documentos](/docs/services/Cloudant/index.html)!
