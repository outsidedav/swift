---

copyright:
  years: 2018
lastupdated: "2018-08-17"

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

Acesse o Catálogo e crie uma instância do API Connect para gerenciar suas APIs.

Use `Menu->APIs` para acessar o console do API Connect Management.

![API Connect](images/apiconnect.png)

Se estiver definindo seu próprio contrato de API antes de iniciar o desenvolvimento de backend e de front-end, use as ferramentas do API Connect para acelerar esse processo. É possível trabalhar com a sua equipe de desenvolvimento digital para construir e definir o contrato de API entre o iOS App e a lógica de backend. Essa lógica pode ser entregue usando o [{{site.data.keyword.openwhisk}}](/docs/openwhisk/index.html) ou por meio do [tempo de execução do Swift](/docs/runtimes/swift/index.html) com Kubernetes ou [Cloud Foundry](/docs/cloud-foundry/index.html).

Depois que sua API é definida, é possível definir as Open API Specifications (Swagger) em várias ferramentas diferentes:

- [ Editor Swagger ](http://editor.swagger.io/)
- [API Designer](https://www.ibm.com/support/knowledgecenter/en/SSFS6T/com.ibm.apic.toolkit.doc/task_apionprem_composing_apis.html)
- [ Loopback ](https://loopback.io/)

## Definindo sua API Gerenciada

É possível definir um proxy de API que gerencia o gateway da API entre o aplicativo cliente e a lógica de backend. Use as etapas a seguir para criar um proxy usando YAML ou JSON da Open API Specification (documento Swagger). 

1. Abra o console `Menu -> APIs` e clique no Proxy da API.
2. Clique em **Importação de definição de API YAML ou JSON**.
3. Selecione o arquivo YAML ou JSON que você criou anteriormente.
4. Salvar e Expor.

É necessário configurar o Terminal externo para apontar para a URL que se vincula ao aplicativo de lógica de backend. 

## Criando um Backend de Swift

É possível criar seu app Swift de backend com base nessa API. 

No Apple Development Console, execute as seguintes etapas:

1. Selecione  ** Kits de Inicialização **.
2. Clique em  ** Criar Aplicativo **.
3. Selecione  ** Swift **  como o idioma.

Selecione o arquivo YAML e JSON e, em seguida, clique em **Criar**. O app Swift de backend é criado.

Em seguida, é possível **fazer download** do Código ou **Implementar na nuvem** e clonar seu repositório GIT na sua máquina local. É possível seguir as instruções no Guia de Conhecimento para abrir o app do lado do servidor no XCode.

Na pasta **Origem**, é possível ver uma rota que define o arquivo Swift que criou os terminais REST que são mapeados para a API. 

Consulte o exemplo a seguir que usa o PetStore Open API:
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

Para consumir a API de backend no iOS App, crie um kit do iniciador do Mobile usando o Apple Console. Usando a visualização Starter Kit, crie um kit do iniciador do iOS Swift de qualquer tipo.

Clique em **Incluir recurso** e selecione uma API. 

![Diálogo da API](../images/apidialog.png)

A API é incluída no iOS App. Se você *fizer download* do código para o app, verá uma pasta incluída nas pastas de Origem do iOS que é nomeada após a API.

Siga as etapas do Guia de Conhecimento para executar `pod update` de todos os SDKs dependentes no iOS App. 

O iOS App inclui uma pasta que contém a ligação de SDK gerada para a API. Essa pasta inclui as três subpastas a seguir: `Assets`,`Source` e `Docs`. 

![iOS Folder](../images/sdkfolder.png)

A pasta `Assets` contém o arquivo que gerencia a URL para sua API, que, por padrão, é `localhost:3000`. Deve-se mudar o valor para referenciar a Rota da API. A definição de API contém uma seção Nome e Rota da API. Clique no **ícone Copiar** no término da rota, para copiar a URL. Verifique se a opção *Expor API gerenciada* está ativada para permitir que clientes externos façam chamadas API.

![Rota da API](../images/apiroute.png)  

Abra o arquivo `PLIST` e substitua o valor do host pelo valor copiado da rota de API que permite que o SDK chame a API para o {{site.data.keyword.cloud_notm}}.

## Documentação

Quando o SDK é incluído no projeto iOS App, um arquivo *README.html* fica disponível na **pasta Docs**. Abra a pasta Docs em um navegador Externo e leia as instruções sobre como usar seu projeto.

## Recreando o SDK após a Alteração de API

Se as mudanças de API ou novos recursos ficarem disponíveis, e o {{site.data.keyword.openwhisk}} for incluído, será possível recriar o SDK do cliente usando o comando `ibmcloud sdk`. Para obter mais informações, exemplos e ajuda de sintaxe, verifique a documentação do [Gerador de SDK](/docs/cli/sdk/index.html).

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

    O SDK é recriado no diretório de projeto do iOS App para que você possa continuar a trabalhar com sua API.

## Referência

O exemplo de SDK a seguir é criado para o {{site.data.keyword.openwhisk_short}} por meio do Starter Kit. É possível ver cada uma das Ações e os fragmentos de código do Swift que podem ser incluídos no iOS App.

### Métodos de API Padrão
 * [ ` getCreate ` ](#getCreate)
 * [ ` getDelete ` ](#getDelete)
 * [ ` getDeleteall ` ](#getDeleteall)
 * [`getRead`](#getRead)
 * [ ` getReadall ` ](#getReadall)
 * [ ` getUpdate ` ](#getUpdate)

### Usando  ` getCreate `
{: #getCreate}

```swift
public static func getCreate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getCreate `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getCreate `

Nenhuma autenticação necessária

### Exemplo que usa `getCreate`
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
{: #getDelete}

```swift
public static func getDelete(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getDelete `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getDelete `

Nenhuma autenticação necessária

### Exemplo que usa  ` getDelete `
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
{: #getDeleteall}

```swift
public static func getDeleteall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getDeleteall `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getDeleteall `

Nenhuma autenticação necessária

### Exemplo que usa `getDeleteall`

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
{: #getRead}

```swift
public static func getRead(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getRead `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getRead `

Nenhuma autenticação necessária

### Exemplo que usa  ` getRead `
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
{: #getReadall}

```swift
public static func getReadall(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para  ` getReadall `

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getReadall `

Nenhuma autenticação necessária

### Exemplo que usa  ` getReadall `
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
{: #getUpdate}

```swift
public static func getUpdate(completionHandler: @escaping (_ response: Response?, _ error: Error?) -> Void)-> Vazio
```
{: codeblock}

#### Parâmetros para `getUpdate`

- ** completionHandler **  (obrigatório)
    - O encerramento usa como argumentos `Response?` e  ` Erro? `.

### Autenticando com  ` getUpdate `

Nenhuma autenticação necessária

### Exemplo que usa  ` getUpdate `
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

