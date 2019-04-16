---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-21"

keywords: foodtrackerbackend, kitura swift, urlsession sdk, alamofire, restkit, kiturakit, kitura, xcode kitura, meals swift, rest api kitura, rest api swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Criando um app com Kitura
{: #kitura}

[Kitura](http://www.kitura.io){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") é uma estrutura do Swift do lado do servidor para construir back-ends do iOS e aplicativos da web. Essa estrutura cria APIs de REST que podem ser chamadas por meio do aplicativo iOS usando SDKs do URLSession como Alamofire, RestKit ou o SDK do [KituraKit](https://github.com/ibm-swift/kiturakit){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") fornecido pelo próprio Kitura.

O Kitura é capaz de se integrar com todos os serviços e recursos que são fornecidos pelo {{site.data.keyword.cloud}}, incluindo o {{site.data.keyword.appid_short}}, o {{site.data.keyword.mobilepushshort}} e o {{site.data.keyword.mobileanalytics_short}}, bem como os bancos de dados, o aprendizado de máquina e outros serviços. O Kitura pode, então, ser implementado e escalado automaticamente usando as plataformas Cloud Foundry ou Docker (baseadas no Kubernetes) no {{site.data.keyword.cloud}}.

O Kitura fornece uma [interface da linha de comandos (CLI)](https://www.kitura.io/guides/kituracli/gettingstarted.html){: new_window} do `kitura` ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") que simplifica a criação, a construção, o teste e a implementação de aplicativos do Kitura. Os aplicativos construídos usando a CLI do Kitura incluem suporte completo para implementação em qualquer nuvem que suporte tecnologias Cloud Foundry, Docker e Kubernetes. No entanto, caso você esteja construindo especificamente para o {{site.data.keyword.cloud_notm}}, é recomendável usar o IBM Apple Development Console no navegador ou usar o {{site.data.keyword.dev_cli_notm}}. Além disso, enquanto ambos os métodos compartilham a tecnologia subjacente, o Apple Development Console e o IBM Developer Tools criam um app hospedado e um pipeline de implementação para você, assim como provisiona os serviços dos quais o seu aplicativo precisa.

## Antes de começar
{: #prereqs-kitura}

Primeiro, verifique se os pré-requisitos a seguir estão prontos para execução:  

* iOS 11.0 +  
* Xcode 9.0  
* Swift 4.0 +  
* CocoaPods  

## Etapa 1. Criando um app do Kitura usando o navegador
{: #create_kitura}

1. Acesse a seção Starter Kits do Apple Development Console. Selecione um iniciador predefinido, como o "Swift for Backend for Frontend API" ou crie um app customizado usando o bloco **Criar app**. Clique em **Criar app**.

2. Forneça um nome ao seu app e selecione em que local você deseja que o seu app seja implementado. Se você não estiver seguro de onde o aplicativo será implementado, use os valores padrão, pois eles poderão ser mudados posteriormente.

3. Selecione o idioma Swift. Um app do Swift do lado do servidor é criado. Também são exibidas as opções de Host e Domínio, que formam a URL para o aplicativo. Se você não estiver seguro, use os padrões. Também é possível fornecer o seu próprio domínio customizado por meio de um provedor de domínio no qual o aplicativo deve residir.

4. É possível fornecer uma definição OpenAPI (também conhecida como Swagger) para a API REST que você deseja construir. Se você tiver essa definição, será criada uma API de REST que inclui as funções de manipulador correspondentes no Kitura. Se não tiver uma definição de OpenAPI, não se preocupe, pois o Kitura facilita a criação manual das APIs de REST usando suas APIs do Roteador.

5. Clique em **Criar app**.

Um app é criado, mas um que ainda não usa nenhum serviço adicional. É possível incluir serviços clicando no botão **Incluir serviço** ou **Fazer download do código** para obter o código para o app. Também é possível incluir serviços facilmente em um app existente.

## Etapa 2. Incluindo Serviços
{: #add_services-kitura}

1. Clique em **Incluir serviço** para incluir serviços. As categorias de serviço são exibidas. Por exemplo, selecione **Dados** para examinar os bancos de dados disponíveis e selecione **Cloudant NoSQL DB**.
2. Selecione um plano de precificação para o serviço, por exemplo, Lite, e clique em **Criar**.

Uma instância do serviço é criada que fornece credenciais para o aplicativo e, em alguns casos, inclui o código necessário em seu app para incluir a conexão correta com o serviço. Agora é possível incluir mais serviços usando o botão **Incluir serviço** ou clicando em **Fazer download do código** para obter o código para o app.

Após fazer download de seu app, será possível começar a trabalhar com ele.

## Etapa 3. Desenvolvendo o aplicativo com o Xcode
{: #develop_xcode-kitura}

Após fazer download de seu código do app, será possível modificá-lo e desenvolvê-lo usando o Xcode e, em seguida, fazer upload de seu aplicativo modificado para implementação na nuvem.

1. Crie um projeto Xcode.  
  Antes que possa usar seu projeto no Xcode, deve-se criar um arquivo de projeto Xcode usando o comando do Swift Package Manager. Execute o comando a seguir por meio do diretório raiz de seu projeto transferido por download, em que o arquivo `Package.swift` reside:
  ```
  geração rápida de pacote-xcodeproj
  ```
  {: codeblock}

  Um projeto Xcode é criado no mesmo diretório que poderá, então, ser aberto.

2. Configure o destino Xcode para o projeto.  
  Para executar o servidor Kitura, deve-se editar o esquema clicando na seção **project_name-Package** na barra de ferramentas e selecionando o destino **project_name** no menu. Verifique se o dispositivo de destino está configurado como **Meu Mac**.

3. Execute o servidor Kitura localmente. 
  Clique em **Executar** ou use o atalho de teclas `?+R` para iniciar o servidor Kitura. Depois que o servidor é iniciado, é possível verificar se as seguintes URLs padrão do Kitura estão em execução:
  * Monitoramento de Kitura:  [ http://localhost: 8080/swiftmetrics-dash/ ]()
  * Verificação de Funcionamento Kitura:  [ http://localhost: 8080/health ]()

## Etapa 4. Incluindo APIs de REST
{: #add_restapi-kitura}

Um servidor Kitura de estrutura básica é criado, mas ele não fornece nenhuma API de REST que pode ser usada por um aplicativo iOS. Inclua APIs de REST no Kitura com a codificação mínima. Use as etapas a seguir para incluir uma API de REST para as solicitações `GET` em `/meals`, que foi projetada para retornar os objetos `Meal` que são armazenados pelo servidor Kitura.

1. Inclua uma estrutura `Meal` no arquivo `Sources/Application/Application.swift`:  
  Antes da declaração da classe `App`, inclua o seguinte para criar uma estrutura Meal que esteja em conformidade com o protocolo Codable:  
  ```swift
	struct Meal: Codable {    
	    var name: String
	    var photo: Data
	    var rating: Int
	}
  ```
  {: codeblock}

2. Inclua um Dicionário no arquivo `Sources/Application/Application.swift` para armazenar objetos `Meal` incluindo `let cloudEnv = CloudEnv()` na seção de código a seguir:
  ```swift
  private var mealStore: [String: Meal] = [:]
  ```
  {: codeblock}

3. Inclua um manipulador para solicitações `GET` em `/meals` no arquivo `Sources/Application/Application.swift`, incluindo o seguinte código na função `postInit()`:  
  ```swift
  router.get ("/refeições", manipulador: loadHandler)
  ```
  {: codeblock}

4. Implemente a função loadHandler no arquivo `Sources/Application/Application.swift` incluindo o código a seguir como outra função na classe `App`:
  ```swift
  func loadHandler(completion: ([Meal]?, RequestError?) -> Void ) {
      let meals: [Meal] = self.mealStore.map({ $0.value })
    completion(meals, nil)
  }
  ```
  {: codeblock"}

Agora você tem uma API de REST para solicitações `GET` em `/meals` que responde com uma matriz de `Meals` ou com um erro.

5. Execute o projeto.  
  Clique em **Executar** ou use o atalho de teclas `?+R`. Se solicitado, selecione **Permitir conexões de rede recebidas**.

6. Teste a API de REST usando uma solicitação `GET`, que corresponde a como os navegadores da web solicitam dados de um servidor. 

  É possível testar a API de REST usando a URL a seguir:  
  ```swift
  * `GET /meals`:	[http://localhost:8080/meals]()
  ```
  {: codeblock}

  Uma matriz vazia é retornada `[]` porque nenhum objeto `Meal` está armazenado atualmente no `mealStore`. 

É possível usar o tutorial [FoodTracker Backend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), que o ajuda a construir um conjunto de APIs de REST para armazenamento, busca e exclusão de objetos `Meal` de um aplicativo iOS, incluindo o armazenamento dos dados em um banco de dados.
{: tip}

## Etapa 5. Instalando o KituraKit em seu aplicativo iOS
{: #install-kiturakit}

As APIs de REST construídas usando o servidor Kitura são APIs padrão da web, utilizáveis por meio de qualquer aplicativo, independentemente da biblioteca de cliente usada ou da linguagem em que o cliente é gravado. Isso significa que é possível usar Alamofire, RestKit ou URLSession para fazer conexões com o servidor. O Kitura também fornece um conector de cliente otimizado sob medida para simplificar a chamada de suas APIs de REST do iOS, na forma de KituraKit. 

O KituraKit fornece uma imagem de espelho das APIs do manipulador do roteador usadas no Kitura, possibilitando compartilhar tipos de Swift entre o cliente e o servidor com pouco esforço. Esse recurso remove a necessidade de realizar explicitamente a codificação e a decodificação de JSON dos dados que são enviados ou recebidos do servidor.

As etapas a seguir mostram como instalar o KituraKit no aplicativo iOS e utilizá-lo para chamar a API de REST `GET /meals` criada usando o Kitura:

1. Crie um Podfile na raiz de seu diretório de aplicativos iOS, se ainda não tiver um:
  ```
  pod init
  ```
  {: codeblock}

2. Edite o Podfile para configurar uma plataforma global do iOS 11 para seu projeto, substituindo a linha a seguir:
  ```pod
  # platform :ios, '9.0'
  ```
  {: codeblock}

  Com o código a seguir:
	```pod
  platform :ios, '11.0'
  ```
  {: codeblock}

3. Inclua o KituraKit no Podfile incluindo o seguinte em `# Pods for <application name>`:
  ```pod
  pod 'KituraKit', :git => 'https://github.com/IBM-Swift/KituraKit.git', :branch => 'pod'
  ```
  {: codeblock}

4. Salve o Podfile e instale o KituraKit usando o comando a seguir:
  ```
  pod install
  ```
  {: codeblock}

5. Reabra o aplicativo em Xcode, abrindo a área de trabalho (não o projeto). Agora é possível incluir a mesma definição `Meal` no aplicativo iOS, conforme usado no servidor Kitura:
  ```swift
  struct Meal: Codable {    
      var name: String
      var photo: Data
      var rating: Int
  }
  ```
  {: codeblock}

  And use the following code to make a connection to the Kitura server:
  
  ```swift
  import KituraKit
  
  /* Connect to the Kitura server on http://localhost:8080 */
  guard let client = KituraKit(baseURL: "http://localhost:8080") else {
      print("Error creating KituraKit client")
      return
  }
  
  /* Make a request to `GET /meals`, expecting a [Meal] or a RequestError */
  client.get("/meals") { (meals: [Meal]?, error: RequestError?) in
      /* Check for an error */
      guard error == nil else {
          print("Error saving meal to Kitura: \(error!)")
          return
      }
      /* Check for meals */
      guard let meals = meals else {
          print("Received Meals!")
          return
      }
  }
  ```
  {: codeblock}

O servidor Kitura é chamado usando `client.get("/meals")` com um retorno de chamada que corresponde aos parâmetros do manipulador de conclusão do Kitura.

É possível usar o tutorial [FoodTrackerBackend](https://github.com/IBM/FoodTrackerBackend){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), que o ajuda a construir um conjunto de APIs de REST para armazenamento, busca e exclusão de objetos `Meal` de um aplicativo iOS, incluindo o armazenamento dos dados em um banco de dados.
{: tip}

## Próximas etapas
{: #next-kitura notoc}

Agora que você tem um servidor Kitura que fornece uma API de REST que pode ser chamada pelo seu
aplicativo iOS, está pronto para implementar seu servidor no {{site.data.keyword.cloud_notm}}. [Implementações](/docs/swift?topic=swift-deploy_apps-swift#deploy_apps-swift) podem ser feitas usando contêineres com Kubernetes, contêineres seguros, o Cloud Foundry, o Cloud Foundry Enterprise Environment ou instâncias virtuais.
