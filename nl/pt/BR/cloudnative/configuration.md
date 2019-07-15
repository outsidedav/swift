---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: swift-cfenv, service bindings swift, environment swift, swift configuration, cloudenvironment swift, VCAP_SERVICES swift, swift credentials

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Configurando o ambiente Swift
{: #configuration}

O desenvolvimento de nuvem nativa tem duas práticas estritamente relacionadas que intersectam como você manipula os dados de configuração. A primeira é que se deve construir artefatos imutáveis para minimizar a probabilidade de que erros sejam introduzidos à medida que seu aplicativo percorre do desenvolvimento para a produção. A segunda é que você deve se esforçar por paridade entre os ambientes de desenvolvimento e de produção, para evitar problemas criados pelo código específico do ambiente. 

O gerenciamento de configuração de serviço e de credenciais (ligações de serviço) varia entre as plataformas. O Cloud Foundry armazena detalhes de ligação de serviços em um objeto JSON em sequência que é passado para o aplicativo como uma variável de ambiente `VCAP_SERVICES`. O Kubernetes armazena ligações de serviços como atributos de JSON ou simples sequenciados em `ConfigMaps` ou `Secrets`, que podem ser passados para o aplicativo conteinerizado como variáveis de ambiente ou montados como um volume provisório. O desenvolvimento local tem a sua própria configuração, pois o teste local é geralmente um derivado simplificado do que está sendo executado na nuvem. Trabalhar por essas variações de uma maneira móvel sem ter caminhos de código específicos do ambiente pode ser desafiador.

É possível seguir as diretrizes simples para ajudar a gravar aplicativos móveis e utilitários que podem ser usados para encapsular ligações de serviço de descoberta (ou outra configuração) em locais específicos do ambiente. Se você precisa incluir suporte de nuvem em aplicativos existentes ou criar apps com kits do iniciador, o objetivo será fornecer portabilidade para os apps Swift para que sejam usados em muitas plataformas de implementação.

## Incluindo  {{site.data.keyword.cloud_notm}}  nos aplicativos Swift existentes
{: #addcloud-env}

O caminho para os valores do ambiente de abstração pode ser diferente de um ambiente de nuvem para outro. A biblioteca do [CloudEnvironment](https://github.com/IBM-Swift/CloudEnvironment){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") abstrai a configuração do ambiente e as credenciais de vários provedores em nuvem para que o seu app do Swift possa acessar consistentemente as informações sendo executado localmente ou no Cloud Foundry, no Cloud Foundry Enterprise Environment, no Kubernetes, no {{site.data.keyword.openwhisk}} ou em instâncias virtuais. A abstração de credenciais é fornecida pela biblioteca do `CloudEnvironment`, que usa internamente [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para a configuração do Cloud Foundry e a [Configuração](https://github.com/IBM-Swift/Configuration){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") como um gerenciador de configuração.

Com o `CloudEnvironment`, é possível abstrair detalhes de baixo nível do código-fonte do aplicativo definindo uma chave de consulta que seu aplicativo Swift pode usar para procurar seu valor correspondente.

A biblioteca `CloudEnvironment` fornece uma chave de consulta consistente que pode ser usada no código-fonte. Em seguida, a biblioteca procura em uma matriz de padrões de procura para localizar um objeto JSON com os atributos de configuração ou as credenciais de serviço. 

### Incluindo o pacote CloudEnvironment no aplicativo Swift
{: #add-cloudenv}

Para usar o pacote `CloudEnvironment` em seu aplicativo Swift, especifique-o na seção **dependências:** de seu arquivo `Package.swift`:
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
{: #access-credentials}

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

Esse exemplo fornece acesso aos conjuntos de credenciais para serviços, que agora podem ser usados para inicializar conexões com esses [serviços suportados ou um dicionário genérico](https://github.com/IBM-Swift/CloudEnvironment#supported-services){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"). Verifique [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para a configuração específica do Cloud Foundry e [detalhes de configuração](https://github.com/IBM-Swift/Configuration){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") sobre o carregamento de dados de configuração.

## Entendendo credenciais de serviço
{: #service_creds}

A biblioteca `CloudEnvironment` usa um arquivo denominado `mappings.json`, localizado no diretório `config`, para comunicar onde as credenciais são armazenadas para cada serviço. O arquivo `mappings.json` suporta a procura de valores que usam os três tipos padrão de procura a seguir:
- **`cloudfoundry`** - Um tipo de padrão usado para procurar um valor na variável de ambiente de serviços do Cloud Foundry (`VCAP_SERVICES`). Para
o Cloud Foundry Enterprise Edition, veja este [tutorial de introdução](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)
para obter mais informações.
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

Neste exemplo, `cloudant-credentials` e `object-storage-credentials` são as chaves de consulta usadas pelo aplicativo Swift para consultar as credenciais correspondentes. De acordo com a matriz de padrões de procura, o gerenciador de configuração primeiro procura `my-awesome-cloudant-db` em `VCAP_SERVICES` e, em seguida, procura por `my_awesome_cloudant_db_credentials` como uma variável de ambiente. Se ela não estiver definida, ele procurará o conteúdo de `my-awesome-object-storage-credentials.json` na pasta `localdev`. 

Quando o aplicativo é executado localmente, ele pode usar credenciais que são armazenadas em um arquivo, como `localdev/my-awesome-object-storage-credentials.json` conforme mostrado no exemplo anterior. As informações de conexão para serviços que você deseja acessar localmente, como nome de usuário, senha e nome do host, devem ser todas armazenadas nesse arquivo. 

Por motivos de segurança, os arquivos de credencial não pertencem aos repositórios. No exemplo anterior, uma pasta `localdev` foi usada para armazenar credenciais locais, portanto, deve-se incluir essa pasta no arquivo `.gitignore` para evitar uma confirmação acidental. Se estiver usando um app do Kit do Iniciador, essa pasta será criada para você e estará presente no arquivo `.gitignore`.

Para obter mais informações sobre o arquivo `mappings.json`, efetue check-out da seção [Entendendo a credencial de serviço](#service_creds).

## Usando o gerenciador de configuração do Swift por meio de apps do kit do iniciador
{: #configmanager-swift}

Apps do Swift que são criados com [kits do iniciador](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") são fornecidos automaticamente com as credenciais e a configuração necessárias para serem executados localmente e também em muitos destinos de implementação de nuvem, como [Kubernetes](/docs/containers?topic=containers-getting-started), [Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf), [{{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-about), [Servidor virtual (VSI)](/docs/vsi?topic=virtual-servers-getting-started-tutorial) ou [{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-getting_started).

  A implementação do VSI está disponível para alguns kits do iniciador. Para usar esse recurso, acesse o [painel do {{site.data.keyword.cloud_notm}}](https://{DomainName}) e clique em **Criar um app** no ladrilho **Apps**.
  {: note}

A criação básica do gerenciador de configuração pode ser localizada em `Sources/Application/Application.swift`. Ao criar um app do kit do iniciador baseado em Swift com serviços, uma pasta `config` e o arquivo `mappings.json` são criados para você. Se você fizer download de seu app, a pasta `config` incluirá um arquivo `localdev-config.json` que tem todas as credenciais para seus serviços e estará presente no arquivo `.gitignore`.

## Próximas etapas
{: #next-configß notoc}

Efetue check-out de nossas três bibliotecas para ajudar seus aplicativos a se abstraírem de seus ambientes:

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
* [Configuração](https://github.com/IBM-Swift/Configuration){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
