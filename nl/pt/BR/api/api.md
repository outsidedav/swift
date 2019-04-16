---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift api connect, swagger swift, open api swift, api designer, loopback swift api, create swift backend, swift api parameters, swift api reference

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Incluindo APIs em apps iOS
{: #api_connect}

É possível usar o API Connect para gerenciar APIs no {{site.data.keyword.cloud}}, caso elas sejam mantidas dentro ou fora do {{site.data.keyword.cloud_notm}}. Aprenda a gerenciar suas APIs para que possa controlar o uso, aumentar a adoção e controlar as estatísticas.

## Criando uma Instância do API Connect
{: #create-apiconnect}

Acesse [Catálogo](https://cloud.ibm.com/catalog/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") e crie uma instância do API Connect para gerenciar as suas APIs.

Use `Menu->APIs` para acessar o console do API Connect Management.

![API Connect](images/apiconnect.png)

Se você estiver definindo seu próprio contrato de API antes de iniciar o desenvolvimento de back-end e front-end, use as ferramentas do API Connect para acelerar esse processo. É possível trabalhar com a sua equipe de desenvolvimento digital para construir e definir o contrato de API entre o iOS App e a lógica de backend. Essa lógica pode ser entregue usando o [{{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-index#index) ou por meio do [tempo de execução do Swift](/docs/runtimes/swift?topic=Swift-swift_runtime#swift_runtime) com Kubernetes ou [Cloud Foundry](/docs/cloud-foundry?topic=cloud-foundry-about#about).

Depois que sua API é definida, é possível definir as Open API Specifications (Swagger) em várias ferramentas diferentes:

- [Editor do Swagger](http://editor.swagger.io/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
- [Designer de API](https://www.ibm.com/support/knowledgecenter/en/SSFS6T/com.ibm.apic.toolkit.doc/task_apionprem_composing_apis.html){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
- [Loopback](https://loopback.io/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")

## Definindo a sua API gerenciada
{: #define-apiconnect}

É possível definir um proxy de API que gerencia o gateway da API entre o aplicativo cliente e a lógica de backend. Use as etapas a seguir para criar um proxy usando YAML ou JSON da Open API Specification (documento Swagger). 

1. Abra o console `Menu -> APIs` e clique no Proxy de API.
2. Clique em **Importação de definição de API YAML ou JSON**.
3. Selecione o arquivo YAML ou JSON que você criou anteriormente.
4. Salvar e Expor.

É necessário configurar o Terminal externo para apontar para a URL que se vincula ao aplicativo de lógica de backend. 

## Criando um back-end do Swift
{: #create-backend-apiconnect}

É possível criar seu app Swift de back-end com base nessa API. 

No [Console do Apple Development](https://cloud.ibm.com/developer/appledevelopment/dashboard){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), execute as etapas a seguir:

1. Selecione **Kits de Inicialização**.
2. Clique em **Criar App**.
3. Selecione **Swift** como o idioma.

Selecione o arquivo YAML e JSON e, em seguida, clique em **Criar**. O app Swift de backend é criado.

Em seguida, é possível **fazer download** do Código ou **Implementar** e clonar o seu repositório GIT para a sua máquina local. É possível seguir as instruções no Guia de conhecimento para abrir o app do lado do servidor no Xcode.

Na pasta **Origem**, é possível ver uma rota que define o arquivo Swift que criou os terminais REST que são mapeados para a API. 

Consulte o exemplo a seguir que usa a API do Open `PetStore`:
```swift
importar Kitura de Importação KituraContracts

func initializePet_Routes (app: App) {
    app.router.post("\(basePath)/pet") { request, response, next in
        response.send(json: [:])
        next() }

    app.router.put("\(basePath)/pet") { request, response, next in
        response.send(json: [:])
        next() }

    app.router.get("\(basePath)/pet/findByStatus") { request, response, next in
        response.send(json: [:])
        next() }

    app.router.get("\(basePath)/pet/findByTags") { request, response, next in
        response.send(json: [:])
        next() }

    app.router.get("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next() }

    app.router.post("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next() }

    app.router.delete("\(basePath)/pet/:petId") { request, response, next in
        response.send(json: [:])
        next() }

    app.router.post("\(basePath)/pet/:petId/uploadImage") { request, response, next in
        response.send(json: [:])
        next ()
    }
}
```
{: codeblock}

Depois que a API for definida usando o {{site.data.keyword.openwhisk_short}} ou um tempo de execução de Swift de pilha integral e a definição de API Connect for criada, será possível consumir a API nos Apps iOS.

## Consumindo a API em um app iOS App Mobile
{: #consume-apiconnect}

Para consumir a API de backend no iOS App, crie um kit do iniciador do Mobile usando o Apple Console. Usando a visualização Starter Kit, crie um kit do iniciador do iOS Swift de qualquer tipo.

Clique em **Incluir serviço** e selecione uma API. 

![Diálogo da API](../images/apidialog.png)

A API é incluída no iOS App. Se você *fizer download* do código para o app, verá uma pasta incluída nas pastas de Origem do iOS que é nomeada após a API.

Siga as etapas do Guia de Conhecimento para executar `pod update` de todos os SDKs dependentes no iOS App. 

O app iOS inclui uma pasta com a ligação de SDK gerada para a API. Essa pasta inclui as três subpastas a seguir: `Assets`,`Source` e `Docs`. 

![iOS Folder](../images/sdkfolder.png)

Na pasta `Assets`, é um arquivo que gerencia a URL para a sua API que, por padrão, é `localhost:3000`. Deve-se mudar o valor para referenciar a Rota da API. A definição de API é composta de uma seção Nome da API e rota. Clique em **Copiar** no final da rota para copiar a URL. Verifique se a opção *Expor API gerenciada* está ativada para permitir que clientes externos façam chamadas API.

![Rota da API](../images/apiroute.png)  

Abra o arquivo `PLIST` e substitua o valor do host pelo valor copiado da rota de API que permite que o SDK chame a API para o {{site.data.keyword.cloud_notm}}.

## Documentação
{: #docs-apiconnect}

Quando o SDK for incluído em seu projeto do app iOS, um arquivo *README.html* estará disponível na pasta `Docs`. Abra a pasta `Docs` em um navegador externo e leia as instruções sobre como usar seu projeto.

## Recriando o SDK após a mudança de API
{: #change-apiconnect}

Se as mudanças de API ou os novos recursos forem disponibilizados e o {{site.data.keyword.openwhisk}} for incluído, será possível recriar o SDK do cliente usando o comando `ibmcloud sdk`. Para obter mais informações, exemplos e ajuda de sintaxe, verifique a documentação do [Gerador de SDK](/docs/cli/sdk?topic=cloud-cli-sdk-cli#sdk-cli).

Para ativar a criação de um SDK, use o arquivo YAML ou JSON da Open API Specification (Swagger). É possível recuperar esse arquivo usando os recursos de gerenciamento de API no {{site.data.keyword.cloud_notm}}. 

1. Navegue para `Menu -> APIs -> Managed APIs`.
2. Selecione a API da qual você deseja recuperar a Open API Specification mais recente. 
3. Em seguida, selecione o menu  ** Explorer ** .

![API Explorer](../images/downloadyaml.png)

4. Selecione o ícone de download para fazer download do yaml para a API e salve esse arquivo no diretório de projeto do iOS App.

5. A próxima etapa é executar o comando da CLI `ibmcloud sdk`. 
    ```
    ibmcloud sdk generate --ios --unzip --output ./MyAppFunctions -f ./mobile-bff-functions-1.0.0.yaml SDKMyFunctions
    ```
    {: codeblock}

    O SDK é recriado em seu diretório de projeto do app iOS para que você possa continuar a trabalhar com a sua API.

## Referência
{: #reference-apiconnect}

O exemplo de SDK a seguir é criado para o {{site.data.keyword.openwhisk_short}} por meio do Starter Kit. É possível ver cada uma das Ações e os fragmentos de código do Swift que podem ser incluídos no iOS App.

### Métodos de API Padrão
{: #default-methods-apiconnect}

 * [ ` getCreate ` ](#getCreate)
 * [ ` getDelete ` ](#getDelete)
 * [ ` getDeleteall ` ](#getDeleteall)
 * [`getRead`](#getRead)
 * [ ` getReadall ` ](#getReadall)
 * [ ` getUpdate ` ](#getUpdate)

### Usando  ` getCreate `
{: #getcreate-apiconnect}

{: #getCreate}

```swift
public static func getCreate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getCreate `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getCreate `
{: #auth-getcreate}

Nenhuma autenticação necessária

### Exemplo que usa `getCreate`
{: #example-getcreate}

```swift
DefaultAPI.getCreate() { (response, error) in
    guard error == nil else {
        imprimir (erro!)
        return } if let status = response?.statusCode {
        status do comutador {
        caso 0: print("Resposta padrão") padrão: print("Resposta: \(response?.responseText)") }
    }
}
```
{: codeblock}

### Usando  ` getDelete `
{: #getdelete}

```swift
public static func getDelete(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getDelete `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getDelete `
{: #auth-getdelete}

Nenhuma autenticação necessária

### Exemplo que usa  ` getDelete `
{: #example-getdelete}

```swift
DefaultAPI.getDelete() { (response, error) in
    guard error == nil else {
        imprimir (erro!)
        return } if let status = response?.statusCode {
        status do comutador {
        caso 0: print("Resposta padrão") padrão: print("Resposta: \(response?.responseText)") }
    }
}
```
{: codeblock}

### Usando  ` getDeleteall `
{: #getdeleteall}

```swift
public static func getDeleteall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getDeleteall `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getDeleteall `
{: #auth-getdeleteall}

Nenhuma autenticação necessária

### Exemplo que usa `getDeleteall`
{: #example-getdeleteall}

```swift
DefaultAPI.getDeleteall() { (response, error) in
    guard error == nil else {
        imprimir (erro!)
        return } if let status = response?.statusCode {
        status do comutador {
        caso 0: print("Resposta padrão") padrão: print("Resposta: \(response?.responseText)") }
    }
}
```
{: codeblock}

### Usando  ` getRead `
{: #getread}

```swift
public static func getRead(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getRead `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getRead `
{: #auth-getread}

Nenhuma autenticação necessária

### Exemplo que usa  ` getRead `
{: #example-getread}

```swift
DefaultAPI.getRead() { (response, error) in
    guard error == nil else {
        imprimir (erro!)
        return } if let status = response?.statusCode {
        status do comutador {
        caso 0: print("Resposta padrão") padrão: print("Resposta: \(response?.responseText)") }
    }
}
```
{: codeblock}

### Usando  ` getReadall `
{: #getreadall}

```swift
public static func getReadall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getReadall `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getReadall `
{: #auth-getreadall}

Nenhuma autenticação necessária

### Exemplo que usa  ` getReadall `
{: #example-getreadall}

```swift
DefaultAPI.getReadall() { (response, error) in
    guard error == nil else {
        imprimir (erro!)
        return } if let status = response?.statusCode {
        status do comutador {
        caso 0: print("Resposta padrão") padrão: print("Resposta: \(response?.responseText)") }
    }
}
```
{: codeblock}

### Usando  ` getUpdate `
{: #getupdate}

```swift
public static func getUpdate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para `getUpdate`

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getUpdate `
{: #auth-getupdate}

Nenhuma autenticação necessária

### Exemplo que usa  ` getUpdate `
{: #example-getupdate}

```swift
DefaultAPI.getUpdate() { (response, error) in
    guard error == nil else {
        imprimir (erro!)
        return } if let status = response?.statusCode {
        status do comutador {
        caso 0: print("Resposta padrão") padrão: print("Resposta: \(response?.responseText)") }
    }
}
```
{: codeblock}

