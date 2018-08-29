---

copyright:
  years: 2018
lastupdated: "2018-08-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: .gif}
{:tip: .tip}

# Criando um banco de dados altamente disponível e seguro

Para aproveitar ao máximo um banco de dados altamente disponível e seguro, integre lógica extra em seu aplicativo. Usando os fragmentos de código fornecidos, é possível criar e acessar um banco de dados MongoDB. 

Atualmente, a linguagem de programação suportada para usar o {{site.data.keyword.ihsdbaas_full}}
é Swift 4.0 com o MongoKitten SDK 4.0.0.

## Etapa 1. Criando um Cluster de Banco de Dados
{: #create_dbcluster}

1. Acesse a tela de configuração de serviço do  {{site.data.keyword.ihsdbaas_full}}  em
https://console.bluemix.net/catalog/services/hyper-protect-dbaas.

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

3. Clique em **Criar**.

  O painel do  {{site.data.keyword.cloud_notm}}  é exibido. Talvez seja necessário atualizar seu navegador para ver o novo cluster, que está listado na seção Serviços.</p></li>

4. Quando você seleciona o serviço, as informações do cluster são exibidas.

5. Na guia Gerenciar das informações do cluster, clique em **ABRIR**.

	O painel do  {{site.data.keyword.ihsdbaas_full}}  é exibido.

6. Obtenha os nomes do host e os números de portas das três instâncias de banco de dados criadas que pertencem a seu cluster de banco de dados. São necessários os nomes de host, os números de porta e as credenciais do usuário para as etapas na seção [Conectando-se ao banco de dados](#connect_db).

## Etapa 2. Criando um projeto usando um kit do iniciador
{: #create_with_starter}

É necessário um kit do iniciador que se baseie na estrutura da web do Swift do lado do servidor Kitura.

![Feature Details](videos/StarterKit.gif){: gif}

Use um projeto existente que tenha sido criado por meio desse kit do iniciador ou crie um novo projeto.

1. Abra o painel {{site.data.keyword.cloud_notm}} App Service em https://console.bluemix.net/developer/appservice/dashboard.

2. Selecione a guia  ** Kits de Inicialização ** .

3. Selecione o kit do iniciador **Swift Kitura** Backend for Frontend. Não o confunda com o Starter Kit básico de app da web do Swift Kitura.
  A página Criar novo projeto é exibida.

4. Insira os detalhes do projeto e clique em **Criar projeto**.
  A página do projeto é exibida.

5. Na página de projetos, clique em **Fazer download do código**.

6. Expanda o arquivo zip transferido por download para o diretório do projeto.

## Etapa 3. Conectando-se ao banco de dados
{: #connect_db}

Para assegurar transferência de dados segura, faça download do arquivo de Autoridade de certificação (CA) de
https://api.hypersecuredbaas.ibm.com/cert.pem, and copy it to your project directory.

1. Mude para o diretório de projeto, que contém os arquivos de código de download expandidos.

2. Crie um arquivo JSON denominado `cred.json` para armazenar suas credenciais de acesso no cluster de banco de dados.

3. Insira os valores obtidos como resultado de [Criando um cluster de banco de dados](#create_dbcluster). Os valores devem ser especificados em uma única linha.

	```hljs
	{
	"uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
	<Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
	/admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
	}
	```
	{: codeblock}

	Onde:

	<table>
	  <tr>
	    <th> Parâmetro </th>
	    <th> Descrição </th>
	  </tr>
	  <tr>
	    <td> &lt;<em>admin_ID</em>&gt; </td>
	    <td> É o ID do usuário do administrador de banco de dados, conforme especificado em [Criando um cluster de banco de dados](#create_dbcluster).
	  </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>admin_pwd</em>&gt; </td>
	    <td> É o ID de usuário da senha do administrador, conforme especificado em [Criando um cluster de banco de dados](#create_dbcluster). </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>Hostname_i</em>&gt; </td>
	    <td> É uma réplica do banco de dados <em>i</em> (<em>i</em>=1,2,3), conforme retornado em [Criando um cluster de banco de dados](create_dbcluster). </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>PortNumber_i</em>&gt; </td>
	    <td> É um número da porta <em>i</em> (<em>i</em>=1,2,3), conforme retornado em [Criando um cluster de banco de dados](#create_dbcluster). </td>
	  </tr>
	  <tr>
	    <td> &lt;<em>CA_file</em>&gt; </td>
	    <td> É o nome de arquivo do arquivo de CA transferido por download. Durante a implementação, ele é copiado para o diretório `/swift-project`.</td>
	  </tr>
	</table>

4. Edite o arquivo `Package.swift` para incluir dependências de pacote para o uso do
MongoKitten SDK.

	a. Na seção de dependências, inclua a linha a seguir:
			```hljs
			 .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
			```
			{: codeblock}

	b. Na seção de destinos, inclua a dependência "MongoKitten" na linha a seguir. **Nota:** os valores devem ser especificados em uma única linha.
			```hljs
			 .target(name: "Application", dependencies: [ "Kitura",
                        				"CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
			```
			{: codeblock}

5. Edite o arquivo `Sources/Application/Application.swift` para inicializar a conectividade com o MongoDB usando MongoKitten.

	a. Importe o MongoKitten SDK:
		```
		import MongoKitten
		```
		{: codeblock}

	b. Inclua a classe  ` ApplicationServices `:

		```hljs
		cclass ApplicationServices {
	    // Service references
	    public let mongoDBService: MongoKitten.Database
	    public let myCredFile = "/swift-project/cred.json"

	    public init () throws {
	        // Read credentials from json file cred.json
	        struct ResponseData: Decodable {
	            var uri: String
	        }
	        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
	        let decoder = JSONDecoder()
	        let jsonData = try decoder.decode(ResponseData.self, from: data!)

	        // Run service initializers
	        let server = try Server(jsonData.uri)
	        mongoDBService = MongoKitten.Database(named: "admin", atServer: 		server)
	    }
		}
		```
		{: codeblock}

	c. Na classe pública `App`, inclua as linhas a seguir para inicializar a conexão com o banco de dados:

		```hljs
		aplicativo de classe pública {
	    ...
	    let services: ApplicationServices

	    public init () throws {
	        // Services
	        services = try ApplicationServices()

	    }
	    ...
    	```
    	{: codeblock}

## Etapa 4. Verificando a Conexão com o Banco de
{: #verify_database}

1. Verifique sua conexão com o banco de dados editando o arquivo `Sources/Application/Application.swift` para incluir um comando para testar a conexão com o banco de dados.
Por exemplo, inclua o comando a seguir em `class ApplicationServices`:

	```hljs
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

```hljs
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## Etapa 5. Incorporando seu Código do Aplicativo
{: #embed_appcode}

Agora é possível incluir seu próprio código do aplicativo no projeto. Para obter mais informações sobre como trabalhar com a API de MongoKitten, consulte http://beta.openkitten.org/tutorials/.

## Etapa 6. Implementando seu aplicativo
{: #deploy_app}

É possível executar o aplicativo localmente com as ferramentas de construção necessárias ou no {{site.data.keyword.cloud_notm}} (Cloud Foundry ou Cluster do Kubernetes) por meio do {{site.data.keyword.dev_cli_notm}}.

É possível executar seu aplicativo localmente no sistema host, no Cloud Foundry ou em um Cluster do Kubernetes.

1. [ Instalar ](/docs/cli/reference/bluemix_cli/get_started.html)  a  {{site.data.keyword.cloud_notm}}  CLI

2. Instale o plug-in de ferramentas do desenvolvedor usando o comando `ibmcloud plugin install dev`.

3. Implemente o aplicativo em um [sistema local](#deploy_local), no [Cloud Foundry](#deploy_cf) ou no [Cluster do Kubernetes](#deploy_cluster).

### Implementando localmente
{: #deploy_local}

1. Assegure-se de que o Docker esteja instalado e em execução em seu sistema host local. É possível fazer download do Docker a partir de https://www.docker.com/community-edition#/download.

2. Alterne para o diretório que contém os arquivos de projeto.

3. Para implementar o aplicativo no computador local, insira os comandos:
	```
	Construção de $ibmcloud dev...
	$ibmcloud dev executar
	```
	{: codeblock}

	Esta etapa constrói seu aplicativo e o executa localmente dentro de um contêiner do Docker.

### Implementando no Cloud Foundry
{: #deploy_cf}

1. Alterne para o diretório que contém os arquivos de projeto.

2. Efetue login na sua conta do IBM Cloud e configure a região como `us-south`, conforme mostrado aqui:
	```hljs 	$ ibmcloud login -a https://api.ng.bluemix.net 	...
	$ ibmcloud target -o &lt;<em>your-organization</em>&gt; -s &lt;<em>your-space</em>&gt;
	```
	{: codeblock}

    **Nota:** emitir o comando `ibmcloud login -a https://api.ng.bluemix.net` configura automaticamente a região para o **sul dos EUA**.

3. Para implementar o aplicativo no Cloud Foundry, insira este comando:
	```
	Implementação de $ibmcloud dev
	```
	{: codeblock}

	Você recebe um link clicável para o local em que seu aplicativo está hospedado.

### Implementando em um Cluster do Kubernetes
{: #deploy_cluster}

1. Crie um cluster do Kubernetes em https://console.bluemix.net/containers-kubernetes/clusters.

2. Clique em  ** Criar Cluster **. A guia Acesso exibe informações sobre como acessar o cluster do Kubernetes criado.

3. Para exibir informações sobre o cluster do Kubernetes, abra o painel do app {{site.data.keyword.cloud_notm}}. O painel exibe uma lista de seus serviços, tais como clusters criados, clusters de banco de dados, apps Cloud Foundry e serviços do Cloud Foundry.

4. Alterne para o diretório que contém os arquivos de projeto.

5. Efetue login em sua conta do {{site.data.keyword.cloud_notm}} e configure a região como sul dos EUA, conforme mostrado aqui:
	```hljs 	$ ibmcloud login -a https://api.ng.bluemix.net 	...
	$ ibmcloud target -o <your-organization> -s <your-space>
	```
	{: codeblock}

	**Nota:** emitir o comando `ibmcloud login -a https://api.ng.bluemix.net` configura automaticamente a região para o **sul dos EUA**.

6. Implemente seu aplicativo no Kubernetes, inserindo este comando:
	```
    $ibmcloud dev implementar o contêiner -t
    ```
    {: codeblock}

	Você é solicitado a fornecer o nome do cluster do Kubernetes e o registro do Docker. Assim que as informações são fornecidas, seu aplicativo é implementado no cluster do Kubernetes.
