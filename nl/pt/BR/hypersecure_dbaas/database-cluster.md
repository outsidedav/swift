---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: swift database, secure database swift, cluster database swift, mongokitten swift, verify database swift, credentials swift, storage api swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: data-image-type='gif'}
{:tip: .tip}

# Criando um banco de dados altamente disponível e seguro
{: #create-database-cluster}

Para aproveitar ao máximo um banco de dados altamente disponível e seguro, integre lógica extra em seu aplicativo. Usando os fragmentos de código fornecidos, é possível criar e acessar um banco de dados MongoDB. 

Atualmente, a linguagem de programação suportada para usar o {{site.data.keyword.ihsdbaas_full}}
é Swift 4.0 com o MongoKitten SDK 4.0.0.

## Etapa 1. Criando um Cluster de Banco de Dados
{: #create_dbcluster}

1. Acesse a tela [configuração de serviço do {{site.data.keyword.ihsdbaas_full}}](https://{DomainName}/catalog/services/hyper-protect-dbaas){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

2. Forneça as seguintes informações:

	<dl>
		<dt>Nome do serviço:</dt>
		<dd>Um nome para o serviço de banco de dados.</dd>

    <dt>Escolha uma região / local para implementar em:</dt>
    <dd>Selecione o data center no qual o banco de dados está localizado.</dd>

    <dt>Selecione um grupo de recursos:</dt>
    <dd>Se nenhum grupo de recursos for selecionável, será possível criar um no painel do IBM Cloud.</dd>

		<dt>Nome do Cluster / Replicaset:</dt>
		<dd>Um nome para o cluster de banco de dados.</dd>

		<dt>Nome Admin do Banco de Dados:</dt>
		<dd>Um ID do usuário para o administrador de banco de dados (DBA).</dd>

		<dt>Senha Admin do Banco de Dados:</dt>
		<dd>Uma senha para o ID de usuário do DBA.</dd>

    <dt>Confirmar Senha Admin do Banco de Dados:</dt>
    <dd>Confirme a senha para o ID de usuário do DBA.</dd>

		<dt>Tipo de banco de dados:</dt>
		<dd>Selecione o tipo de banco de dados. Atualmente, somente o MongoDB é suportado.</dd>

    <dt>Contrato de Licença:</dt>
    <dd>Leia o contrato de licença e marque a caixa para confirmar seu contrato.</dd>

    <dt>Precificação:</dt>
		<dd>A solução atual fornece apenas uma categoria de precificação, que é gratuita. Em versões mais recentes, é possível selecionar a categoria de precificação.</dd>
	</dl>

3. Clique em  ** Criar **.

  O painel do  {{site.data.keyword.cloud_notm}}  é exibido. Talvez seja necessário atualizar seu navegador para ver o novo cluster, que está listado na seção Serviços.</p></li>

4. Quando você seleciona o serviço, as informações do cluster são exibidas.

5. Na guia Gerenciar das informações do cluster, clique em **ABRIR**.

	O painel do  {{site.data.keyword.ihsdbaas_full}}  é exibido.

6. Reúna os nomes de host e os números de porta das três instâncias de banco de dados criadas que pertencem ao cluster do banco de dados. Você precisa dos nomes de host, números de porta e credenciais do usuário para as etapas na seção [Conectando-se ao banco de dados](#connect_db).

## Etapa 2. Criando um projeto usando um kit do iniciador
{: #create_starter}

É necessário um kit do iniciador que se baseie na estrutura da web do Swift do lado do servidor Kitura.

![Feature Details](videos/StarterKit.gif){: gif}

Use um projeto existente que tenha sido criado por meio desse kit do iniciador ou crie um novo projeto.

1. Abra o [painel do {{site.data.keyword.cloud_notm}} App Service](https://{DomainName}/developer/appservice/dashboard){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

2. Selecione a guia  ** Kits de Inicialização ** .

3. Selecione o kit do iniciador **Swift Kitura** Backend for Frontend. Não o confunda com o Starter Kit básico de app da web do Swift Kitura.
  A página Criar novo projeto é exibida.

4. Insira os detalhes do projeto e clique em **Criar projeto**.
  A página do projeto é exibida.

5. Na página de projetos, clique em **Fazer download do código**.

6. Expanda o arquivo compactado para o diretório do projeto.

## Etapa 3. Conectando-se ao banco de dados
{: #connect_db}

Para assegurar a transferência de dados segura, faça download do arquivo de autoridade de certificação (CA) e copie-o para o seu diretório de projeto.

1. Mude para o diretório do projeto no qual você tem os arquivos de código transferidos por download expandidos.

2. Crie um arquivo JSON denominado `cred.json` para armazenar suas credenciais de acesso no cluster de banco de dados.

3. Insira os valores que reunidos das etapas em [Criando um cluster de banco de dados](/docs/swift?topic=swift-create-database-cluster#create_dbcluster). Os valores devem ser especificados em uma única linha.
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

### Descrições de parâmetro de banco de dados
{: #db-parameter-descriptions}

Consulte as descrições de parâmetro de banco de dados a seguir:

* `admin_ID` - o ID do usuário do administrador de banco de dados, conforme especificado em [Criando um cluster de banco de dados](/docs/swift?topic=swift-create-database-cluster#create_dbcluster).
* `admin_pwd` - o ID do usuário da senha do administrador, conforme especificado em [Criando um cluster de banco de dados](/docs/swift?topic=swift-create-database-cluster#create_dbcluster).
* `Hostname_i` - uma réplica do banco de dados *i* (*i*=1,2,3) conforme retornado em [Criando um cluster de banco de dados](/docs/swift?topic=swift-create-database-cluster#create_dbcluster).
* `PortNumber_i` - um número da porta *i* (*i*=1,2,3) conforme retornado em [Criando um cluster de banco de dados](/docs/swift?topic=swift-create-database-cluster#create_dbcluster).
* `CA_file` - é o nome do arquivo de autoridade de certificação transferido por download. Durante a implementação, ele é copiado para o diretório `/swift-project`.

4. Edite o arquivo `Package.swift` para incluir dependências de pacote para o uso do
MongoKitten SDK.

  * Na seção de dependências, inclua a linha a seguir:
   ```swift
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * Na seção de destinos, inclua a dependência "MongoKitten" na linha a seguir. **Nota:** os valores devem ser especificados em uma única linha.
   ```swift
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. Edite o arquivo `Sources/Application/Application.swift` para inicializar a conectividade com o MongoDB usando MongoKitten.

  * Importe o MongoKitten SDK:
    ```swift import MongoKitten
	  ```
	  {: codeblock}

  * Inclua a classe  ` ApplicationServices `:
    ```swift
	  cclass ApplicationServices {
	  /* Service references */
	  public let mongoDBService: MongoKitten.Database
	  public let myCredFile = "/swift-project/cred.json"

    public init () throws {
        /* Read credentials from json file cred.json */
        struct ResponseData: Decodable {
            var uri: String
	        }
	        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
	        let decoder = JSONDecoder()
	        let jsonData = try decoder.decode(ResponseData.self, from: data!)

        /* Run service initializers */
        let server = try Server(jsonData.uri)
        mongoDBService = MongoKitten.Database(named: "admin", atServer: 		server)
    }
	}
	```
	{: codeblock}

  * Na classe pública `App`, inclua as linhas a seguir para inicializar a conexão com o banco de dados:
    ```swift
	  public class App {
	  ...
	  let services: ApplicationServices

	  public init () throws {
	  /* Services */
	  services = try ApplicationServices()
	  }
	  ...
    ```
    {: codeblock}

## Etapa 4. Verificando a Conexão com o Banco de
{: #verify_database}

1. Verifique sua conexão com o banco de dados editando o arquivo `Sources/Application/Application.swift` para incluir um comando para testar a conexão com o banco de dados.
Por exemplo, inclua o comando a seguir em `class ApplicationServices`:

	```swift
		classe ApplicationServices {
		    ...
		    public init () throws {
		        ...
		        let server = try Server(jsonData.uri)
		        mongoDBService = MongoKitten.Database(named: "admin", atServer: server)
		        if server.isConnected {
		            print("Connected to mongodb: " + String(describing: mongoDBService))
		        }
		        ...
		    }
		}
	```
	{: codeblock}

Depois de implementar seu aplicativo na [Etapa 6](#use-step6), a mensagem a seguir será exibida, se
a conexão com o banco de dados for bem-sucedida:

```
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## Etapa 5. Incorporando seu Código do Aplicativo
{: #embed_appcode}

Agora é possível incluir seu próprio código do aplicativo no projeto. Para obter mais informações, consulte a documentação da [API do MongoKitten](http://beta.openkitten.org/tutorials/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Etapa 6. Implementando seu aplicativo
{: #deploy-dbcluster}

É possível executar o aplicativo [localmente](/docs/swift?topic=swift-swift_cli#swift-install-tools) com as ferramentas
de construção necessárias ou implementar no {{site.data.keyword.cloud_notm}}.

Para criar uma cadeia de ferramentas de implementação no painel, clique em **Implementar**. Configure o destino de implementação de acordo com as instruções para o método escolhido:
  * **Implementar no IBM Kubernetes Service**. Essa opção cria um cluster de hosts, chamados de nós do trabalhador, para implementar e gerenciar contêineres de app altamente disponíveis. É possível criar um cluster ou implementar em um cluster existente. Para obter mais informações, consulte [Implementando apps em clusters do Kubernetes](/docs/containers?topic=containers-app).
  * **Implemente no Cloud Foundry**. Essa opção implementa o seu app nativo de nuvem sem você precisar gerenciar a infraestrutura subjacente. Se a sua conta tiver acesso ao {{site.data.keyword.cfee_full_notm}}, será possível selecionar um tipo de implementador de **Nuvem pública** ou **Ambiente corporativo**, que pode ser usado para criar e gerenciar ambientes isolados para hospedar apps do Cloud Foundry exclusivamente para a sua empresa. Para obter mais informações, consulte [Implementando apps no Cloud Foundry Public](/docs/cloud-foundry-public?topic=cloud-foundry-public-deployingapps) e [Implementando apps no {{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-deploy_apps).
  * **Implementar em um Servidor virtual**. Essa opção provisiona uma instância de servidor virtual, carrega uma imagem que inclui o seu app, cria uma cadeia de ferramentas do DevOps e inicia o primeiro ciclo de implementação para você. Para obter mais informações, consulte [Implementando apps em um servidor virtual](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server).
  
