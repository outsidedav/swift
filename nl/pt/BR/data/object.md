---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: object storage swift, static storage swift, file services swift, swift storage class, cos swift, swift data encryption, static swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando o Object Storage para conteúdo estático
{: #object-storage}

O Object Storage é um componente fundamental da computação em nuvem e fornece recursos poderosos para os desenvolvedores de Apple e seus aplicativos. Ao contrário do armazenamento de informações em uma hierarquia de arquivos (como armazenamento de Bloco ou de Arquivo), um armazenamento de objeto consiste apenas nos arquivos e seus metadados, armazenados em coleções conhecidas como depósitos. Por definição, esses objetos são imutáveis, o que os torna perfeitos para dados como imagens, vídeos e outros documentos estáticos. Para dados que mudam frequentemente ou são relacionais, é possível usar os serviços de banco de dados do [Cloudant](/docs/swift/data?topic=swift-cloudant#cloudant) e da [SQL](/docs/swift/data?topic=swift-sql_data#sql_data).

{{site.data.keyword.cos_full_notm}} (COS) é um sistema de armazenamento que pode ser usado para armazenar dados não estruturados que sejam flexíveis, com custo reduzido e escaláveis. Os dados são acessíveis por meio de SDKs ou usando a interface com o usuário da IBM. É possível usar o {{site.data.keyword.cos_full_notm}} para acessar os dados não estruturados por meio de um portal de autoatendimento que é suportado por APIs de RESTful e SDKs. 

Dependendo de suas necessidades, é possível usar o {{site.data.keyword.cos_short}} para os serviços a seguir:

* Repositório de conteúdo
* Backup e Recuperação
* Arquivamento de Dados
* Serviços de arquivo
* Transferência de dados

Quando você criar um depósito, deverá selecionar um nível de resiliência (região cruzada ou regional). Com base em sua seleção, seus dados são dispersos e armazenados em mais de uma localização geográfica.

## API
{: #api-cos}

A API do {{site.data.keyword.cos_full}} é uma API baseada em REST para objetos de leitura e composição. Ela suporta um subconjunto da API do S3 para migração fácil de aplicativos para o {{site.data.keyword.cloud_notm}}. Qualquer S3 SDK pode ser usado para usar o {{site.data.keyword.cos_full}}. Para obter mais informações, consulte a [Referência de API do {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api) integral.

## Protegendo o armazenamento de objetos
{: #security-cos}

{{site.data.keyword.cos_full_notm}}  é altamente seguro. Inicialmente, apenas os proprietários do depósito e do objeto originalmente têm acesso ao serviço {{site.data.keyword.cos_full_notm}} que eles criam. Ele também suporta a autenticação do usuário para acessar os dados. Use mecanismos de controle de acesso, como políticas do depósito, para conceder permissões seletivamente a usuários e aplicativos. É possível transferir com segurança seus dados para o {{site.data.keyword.cos_short}} por meio de terminais SSL usando o protocolo HTTPS. Se precisar de segurança extra, será possível usar a opção Server-Side Encryption (SSE) ou a opção Server-Side Encryption with Customer-Provided Keys (SSE-C) para criptografar dados armazenados em repouso. O {{site.data.keyword.cos_full_notm}} fornece a tecnologia de criptografia para SSE e SSE-C. Como alternativa, é possível usar suas próprias bibliotecas de criptografia para criptografar dados antes de armazená-los no Cloud Object Storage.

É possível usar os mecanismos de controle de acesso a seguir para proteger seus dados no {{site.data.keyword.cos_full_notm}}.

### Políticas do Identity and Access Management (IAM)
{: #iam-cos}

O IAM permite que as organizações com vários funcionários criem e gerenciem vários usuários
sob uma única conta. Com políticas do IAM, as empresas podem conceder aos usuários do IAM controle para seus depósitos do {{site.data.keyword.cos_short}}.

### ACLs (Listas de Controle de Acesso)
{: #acls-cos}

Com as ACLs, é possível conceder permissões específicas (por exemplo, READ, WRITE) para usuários específicos para um depósito individual.

Os dados em repouso são criptografados com criptografia Advanced Encryption Standard (AES) de 256 bits automática do lado do provedor e hash do Secure Hash Algorithm (SHA)-256. Os dados em movimento são protegidos usando a classificação de operadora integrada Segurança da Camada de Transporte (TLS), Secure Sockets Layer (SSL) ou SNMPv3 com criptografia AES.

### Encryption
{: #encryption-cos}

Os dados em repouso são criptografados com criptografia Advanced Encryption Standard (AES) de 256 bits automática do lado do provedor e hash do Secure Hash Algorithm (SHA)-256. Os dados em movimento são protegidos usando a classificação de operadora integrada Segurança da Camada de Transporte/Secure Sockets Layer (TLS/ SSL) ou SNMPv3 com criptografia AES.

A criptografia do lado do servidor está sempre ATIVA para dados do cliente. Comparado com o hashing que é necessário na autenticação e a codificação de apagamento, a criptografia não é uma parte significativa do custo de processamento do Cloud Object Storage.

É possível fornecer sua própria chave para criptografia, pois o SSE-C é suportado no {{site.data.keyword.cos_full_notm}}. No entanto, a responsabilidade está em você gerenciar e fornecer a chave para armazenamento e recuperação dos dados.

## Resiliência
{: #resiliency-cos}

Quando você criar um depósito, deverá selecionar um nível de resiliência (região cruzada ou regional). Com base em sua seleção, seus dados são dispersos e armazenados em mais de uma localização geográfica.

A resiliência regional é para baixa latência e seus dados são distribuídos em três centros dentro de uma única região. Use a resiliência de região cruzada para disponibilidade crítica, pois seus dados são armazenados em três ou mais regiões diferentes. A região cruzada oferece resiliência geográfica e está disponível em mais de um terminal. Considere que seu aplicativo precisa decidir entre os dois.

### Geografias
{: #geographies-cos}

É possível usar o {{site.data.keyword.cos_full_notm}} de qualquer lugar do mundo. É possível escolher onde você deseja armazenar seus dados no momento da criação do depósito. As informações que são armazenadas no IBM COS são criptografadas e dispersas em mais de uma localização geográfica usando tecnologias de armazenamento distribuído que são fornecidas pelo IBM Object Storage System. 

Considere os fatores a seguir para selecionar a localização geográfica de seu armazenamento de objeto quando estiver decidindo entre opções regionais e de região cruzada.

Considerações de localização geográfica:
* Um local que é remoto de suas operações para redundância.
* Um local para tratar os requisitos legais e regulamentares.
* Reduza as latências de acesso a dados.
* Considerar modelos de precificação.

## Casos de Uso e Classes de Armazenamento
{: #usecases-cos}

Dependendo de seu caso de uso, é possível reduzir custos selecionando um plano de serviço que atenda às suas necessidades. As operações de arquivamento que envolvem o acesso mínimo ao armazenamento de objeto não precisam da velocidade e durabilidade de um objeto acessado frequentemente, e essa distinção é refletida no suporte à Classe de armazenamento e no plano de precificação para seus aplicativos. As classes de armazenamento são definidas no nível de depósito, portanto, é possível usar uma combinação de planos para atender às suas necessidades. Crie um depósito que esteja configurado para a classe de armazenamento que você deseja usar.

Mais informações sobre a precificação estão disponíveis na documentação do [{{site.data.keyword.cos_short}} Storage Class](/docs/services/cloud-object-storage/help?topic=cloud-object-storage-billing#ibm-cos-pricing).

### Classes de Armazenamento de Amostra
{: #samples-cos}

- Norma
  
  Esse serviço é para dados não estruturados que requerem acesso frequente, como o DevOps, colaboração e repositórios de conteúdo de ação.

- Segurança
  
  Esse serviço é para cargas de trabalho com dados acessados com pouca frequência, como cargas de trabalho de backup, de archive e de conformidade.

- Área Segura-fria
  
  Essa ação de implementação é ideal para requisitos de acesso mínimos, conformidade de registros históricos e backup de longo prazo.

- Flexível

  Implemente para vários requisitos de acesso a dados e proteja seu orçamento contra flutuações de custo inesperadas. As classes de armazenamento são definidas no nível de depósito. Crie um depósito que esteja configurado para a classe de armazenamento que você deseja usar.


