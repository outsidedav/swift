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

# Configurando o ambiente Swift
{: #configuration}

O desenvolvimento do Cloud Native possui duas práticas estreitamente relacionadas que se cruzam em como você manipula dados de configuração. A primeira é a necessidade de construir artefatos imutáveis para minimizar a probabilidade de inserção de erros à medida que seu aplicativo vai do desenvolvimento para a produção. A segunda é que você deve se esforçar por paridade entre os ambientes de desenvolvimento e de produção, para evitar problemas criados pelo código específico do ambiente. 

O gerenciamento de configuração de serviço e de credenciais (ligações de serviço) varia entre as plataformas. O Cloud Foundry armazena detalhes de ligação de serviços em um objeto JSON sequenciado que é passado para o aplicativo como uma variável de ambiente (`VCAP_SERVICES`). O Kubernetes armazena ligações de serviços como atributos de JSON ou simples sequenciados em `ConfigMaps` ou `Secrets`, que podem ser passados para o aplicativo conteinerizado como variáveis de ambiente ou montados como um volume provisório. O desenvolvimento local tem a sua própria configuração, pois o teste local é geralmente um derivado simplificado do que está sendo executado na nuvem. Trabalhar por essas variações de uma maneira móvel sem ter caminhos de código específicos do ambiente pode ser desafiador.

É possível seguir orientações simples que podem ajudar a gravar aplicativos móveis e alguns utilitários que podem ser usados para encapsular a localização de ligações de serviços (ou outra configuração) em locais específicos do ambiente. Se você precisa incluir suporte à nuvem em aplicativos existentes ou criar apps com Starter Kits, o objetivo é fornecer a portabilidade máxima para os apps, independentemente da plataforma de implementação.

## Incluindo  {{site.data.keyword.cloud_notm}}  nos aplicativos Swift existentes
{: #addcloud-env}

O caminho para a obtenção de determinados valores de ambiente pode ser diferente de um ambiente de nuvem para outro. A biblioteca [CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment.git) abstrai configuração do ambiente e credenciais de vários provedores de nuvem para que seu app Swift possa acessar de forma consistente as informações executando localmente ou no Cloud Foundry, no Kubernetes ou no {{site.data.keyword.openwhisk}}. A abstração de credenciais é fornecida pela biblioteca `CloudEnvironment`, que usa internamente a configuração [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv) para Cloud Foundry e a [Configuração](https://github.com/IBM-Swift/Configuration) como um gerenciador de configuração.

Com a `CloudEnvironment`, é possível abstrair detalhes de baixo nível do código-fonte do aplicativo, definindo uma chave de consulta que seu aplicativo Swift pode alavancar para procurar seu valor correspondente.

A biblioteca `CloudEnvironment` fornece uma chave de consulta consistente que pode ser usada no código-fonte. Em seguida, a biblioteca procura em uma matriz de padrões de procura para localizar o objeto JSON que contém atributos de configuração ou credenciais de serviço. 

### Incluindo o pacote CloudEnvironment no aplicativo Swift
Para alavancar o pacote `CloudEnvironment` no aplicativo Swift, especifique-o na seção **dependências:** do arquivo `Package.swift`:
```swift
.package (url: "https: //github.com/IBM-Swift/CloudEnvironment.git ", de:" 8.0.0 "),
```
{: codeblock}

Em seguida, inclua o código de instrumentação a seguir no aplicativo:
```swift
import CloudEnvironment

let cloudEnv = CloudEnv ()
```
{: codeblock}

### Acessando Credenciais
Agora que a biblioteca `CloudEnvironment` foi inicializada, será possível acessar as suas credenciais, conforme mostrado nos exemplos a seguir:
```swift
let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username, cloudantCredentials.password, cloudantCredentials.url, etc.
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username, objStorageCredentials.password, objStorageCredentials.projectID, etc.

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

// Get a default PORT and URL
let port = cloudEnv.port
let url = cloudEnv.url
```
{: codeblock}

Esse exemplo fornece acesso aos conjuntos de credenciais para serviços, que agora podem ser usados para inicializar conexões com esses [serviços suportados ou um dicionário genérico](https://github.com/IBM-Swift/CloudEnvironment#supported-services). Efetue check-out da configuração específica do [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api) para Cloud Foundry e de [detalhes de configuração](https://github.com/IBM-Swift/Configuration) sobre o carregamento de dados de configuração.

## Entendendo credenciais de serviço
{: #service_creds}

A biblioteca `CloudEnvironment` usa um arquivo denominado `mappings.json`, localizado no diretório `config`, para comunicar onde as credenciais são armazenadas para cada serviço. O arquivo `mappings.json` suporta a procura de valores que usam os três tipos padrão de procura a seguir:
- **`cloudfoundry`** - Um tipo de padrão usado para procurar um valor na variável de ambiente de serviços do Cloud Foundry (`VCAP_SERVICES`).
- **`env`** - Um tipo de padrão usado para procurar um valor que é mapeado para uma variável de ambiente, como no Kubernetes ou Functions.
- **`file`** - Um tipo de padrão usado para procurar um valor em um arquivo JSON. O caminho deve ser relativo à pasta raiz do aplicativo Swift.

A matriz de valores associados a uma chave de consulta é procurada na ordem em que aparece.

A seguir é mostrado um exemplo de um arquivo `mappings.json`:
```javascript
{
    "cloudant-credentials": {
        "credenciais": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-cloudant-db",
                "env:my_awesome_cloudant_db_credentials",
                "file:localdev/my-awesome-cloudant-db-credentials.json"
            ]
        }
    },
    "object-storage-credentials": {
        "credenciais": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-object-storage",
                "env:my_awesome_object_storage_credentials",
                "file:localdev/my-awesome-object-storage-credentials.json"
            ]
        }
    }
}
```
{: codeblock}

Neste exemplo, `cloudant-credentials` e `object-storage-credentials` são as chaves de consulta usadas pelo aplicativo Swift para consultar as credenciais correspondentes. De acordo com a matriz de padrões de procura, o gerenciador de configuração primeiro procura `my-awesome-cloudant-db` em `VCAP_SERVICES` e, em seguida, procura `my_awesome_cloudant_db_credentials` como uma variável de ambiente e, se ela não estiver definida, ele procura o conteúdo de `my-awesome-object-storage-credentials.json` na pasta `localdev`. 

Quando o aplicativo é executado localmente, ele pode usar credenciais armazenadas em um arquivo, como `localdev/my-awesome-object-storage-credentials.json`, conforme mostrado no exemplo anterior. As informações de conexão para serviços que você deseja acessar localmente, como nome de usuário, senha e nome do host, devem ser todas armazenadas nesse arquivo. 

Por motivos de segurança, os arquivos de credenciais não devem ser incluídos em repositórios. No exemplo anterior, uma pasta `localdev` foi usada para armazenar credenciais locais, portanto, deve-se incluir essa pasta no arquivo `.gitignore` para evitar uma confirmação acidental. Se você estiver usando um app Starter Kit, essa pasta será criada para você e estará presente no arquivo `.gitignore`.

Para obter mais informações sobre o arquivo `mappings.json`, efetue check-out da seção [Entendendo a credencial de serviço](configuration.html#service_creds).

## Usando o Gerenciador de configuração do Swift por meio de apps Starter Kit

Os apps Swift criados com [Starter Kits](https://console.bluemix.net/developer/appledevelopment/starter-kits/) vêm automaticamente com as credenciais e a configuração que são necessárias para execução local, e também em muitos ambientes de implementação da Nuvem (CF, K8s, VSI e Functions). A criação básica do gerenciador de configuração pode ser localizada em `Sources/Application/Application.swift`. Ao criar um app Starter Kit baseado em Swift com serviços, uma pasta `config`, contendo um `mappings.json`, é criada para você. Se você fizer download de seu app, a pasta `config` conterá um arquivo `localdev-config.json`, que tem todas as credenciais para seus serviços, e estará presente no arquivo `.gitignore`.

## Próximas Etapas
{: #next notoc}

Efetue check-out de nossas três bibliotecas para ajudar seus aplicativos a se abstraírem de seus ambientes:

* [ CloudEnvironment ](https://github.com/ibm-developer/ibm-cloud-env)
* [ Swift-cfenv ](https://github.com/IBM-Swift/Swift-cfenv)
* [ Configuração ](https://github.com/IBM-Swift/Configuration)
