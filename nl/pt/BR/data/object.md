---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando o Object Storage para conteúdo estático
{: #object}

Object Storage é um componente fundamental da computação em nuvem, fornecendo recursos poderosos para desenvolvedores da Apple e seus aplicativos. Ao contrário do armazenamento de informações em uma hierarquia de arquivos (como armazenamento de Bloco ou de Arquivo), um armazenamento de objeto consiste apenas nos arquivos e seus metadados, armazenados em coleções conhecidas como depósitos. Por definição, esses objetos são imutáveis, o que os torna perfeitos para dados como imagens, vídeos e outros documentos estáticos. Para dados que mudam frequentemente ou são relacionais, é possível usar os serviços de banco de dados [NoSQL](/docs/swift/data/nosql.html), [Cloudant](/docs/swift/data/cloudant.html) e [SQL](/docs/swift/data/sql.html).

{{site.data.keyword.cos_full_notm}} (COS) é um sistema de armazenamento que pode ser usado para armazenar dados não estruturados que sejam flexíveis, com custo reduzido e escaláveis. Os dados são acessíveis por meio de SDKs ou usando a interface com o usuário da IBM. É possível usar o {{site.data.keyword.cos_full_notm}} para acessar seus dados não estruturados por meio de um portal de autoatendimento que é suportado por APIs de Restful e SDKs. 

Dependendo de suas necessidades, é possível usar o {{site.data.keyword.cos_short}} para os serviços a seguir:

* Repositório de conteúdo
* Backup e Recuperação
* Arquivamento de Dados
* Serviços de arquivo
* Transferência de dados

Quando você criar um depósito, deverá selecionar um nível de resiliência (região cruzada ou regional). Com base na seleção, seus dados são dispersos e armazenados ao longo de múltiplos locais geográficos.

## API

A API do {{site.data.keyword.cos_full}} é uma API baseada em REST para objetos de leitura e composição. Ela suporta um subconjunto da API do S3 para migração fácil de aplicativos para o {{site.data.keyword.cloud_notm}}. Qualquer S3 SDK pode ser usado para alavancar o {{site.data.keyword.cos_full}}. Para obter mais informações, consulte a [Referência de API completa do {{site.data.keyword.cos_short}}](docs/services/cloud-object-storage/api-reference/about-compatibility-api.html#about-the-ibm-cloud-object-storage-api)

## Segurança
{: #security}

{{site.data.keyword.cos_full_notm}}  é altamente seguro. Inicialmente, apenas os proprietários do depósito e do objeto originalmente têm acesso ao serviço {{site.data.keyword.cos_full_notm}} que eles criam. Ele também suporta a autenticação do usuário para acessar os dados. Use mecanismos de controle de acesso, como políticas do depósito, para conceder permissões seletivamente a usuários e aplicativos. É possível transferir com segurança seus dados para o {{site.data.keyword.cos_short}} por meio de terminais SSL usando o protocolo HTTPS. Se precisar de segurança extra, será possível usar a opção Server-Side Encryption (SSE) ou a opção Server-Side Encryption with Customer-Provided Keys (SSE-C) para criptografar dados armazenados em repouso. O {{site.data.keyword.cos_full_notm}} fornece a tecnologia de criptografia para SSE e SSE-C. Como alternativa, é possível usar suas próprias bibliotecas de criptografia para criptografar dados antes de armazená-los no Cloud Object Storage.

É possível usar os mecanismos de controle de acesso a seguir para proteger seus dados no {{site.data.keyword.cos_full_notm}}.

**Políticas do Identity and Access Management (IAM)**
O IAM permite que as organizações com vários funcionários criem e gerenciem vários usuários sob uma única conta. Com políticas do IAM, as empresas podem conceder aos usuários do IAM controle para seus depósitos do {{site.data.keyword.cos_short}}.

**Listas de controle de acesso (ACLs)**
Com as ACLs, é possível conceder permissões específicas (por exemplo, LEITURA, GRAVAÇÃO), para usuários específicos de um depósito individual.

Os dados em repouso são criptografados com criptografia Advanced Encryption Standard (AES) de 256 bits automática do lado do provedor e hash do Secure Hash Algorithm (SHA)-256. Os dados em movimento são protegidos usando a classificação de operadora integrada Segurança da Camada de Transporte (TLS), Secure Sockets Layer (SSL) ou SNMPv3 com criptografia AES.

### Encryption

Os dados em repouso são criptografados com criptografia Advanced Encryption Standard (AES) de 256 bits automática do lado do provedor e hash do Secure Hash Algorithm (SHA)-256. Os dados em movimento são protegidos usando a classificação de operadora integrada Segurança da Camada de Transporte/Secure Sockets Layer (TLS/ SSL) ou SNMPv3 com criptografia AES.

A criptografia do lado do servidor está sempre ATIVA para dados do cliente. Em comparação com o hashing necessário na autenticação e a codificação de apagamento, a criptografia não é uma parte significativa do custo de processamento do Cloud Object Storage.

É possível fornecer sua própria chave para criptografia, pois o SSE-C é suportado no {{site.data.keyword.cos_full_notm}}. No entanto, a responsabilidade está em você gerenciar e fornecer a chave para armazenamento e recuperação dos dados.

## Resiliência

Quando você criar um depósito, deverá selecionar um nível de resiliência (região cruzada ou regional). Com base na seleção, seus dados são dispersos e armazenados ao longo de múltiplos locais geográficos.

A resiliência regional é para baixa latência e seus dados são distribuídos em três centros dentro de uma única região. Resiliência de região cruzada se para disponibilidade essencial e seus dados forem armazenados em 3 ou mais regiões diferentes. A região cruzada oferece resiliência geográfica e está disponível em múltiplos terminais. Considere que seu aplicativo precisa decidir entre os dois.

### Geografias

É possível usar o {{site.data.keyword.cos_full_notm}} de qualquer lugar do mundo. É possível escolher onde você deseja armazenar seus dados no momento da criação do depósito. As informações armazenadas no IBM COS são criptografadas e dispersas em vários locais geográficos, usando tecnologias de armazenamento distribuídas fornecidas pelo IBM Object Storage System. 

Considere os fatores a seguir para selecionar o local geográfico de seu armazenamento de objeto, além de decidir entre opções regionais e de região cruzada.

** Considerações de Local **:
* Um local que é remoto de suas operações para redundância.
* Um local para tratar os requisitos legais e regulamentares.
* Reduza as latências de acesso a dados.
* Considerar modelos de precificação.

## Casos de Uso e Classes de Armazenamento

Dependendo de seu caso de uso, é possível reduzir custos selecionando um plano de serviço que atenda às suas necessidades. As operações de arquivamento que envolvem o acesso mínimo ao armazenamento de objeto não precisam da velocidade e durabilidade de um objeto acessado frequentemente, e essa distinção é refletida no suporte à Classe de armazenamento e no plano de precificação para seus aplicativos. As classes de armazenamento são definidas no nível de depósito, portanto, é possível usar uma combinação de planos para atender às suas necessidades. Simplesmente crie um novo depósito que esteja configurado para a classe de armazenamento desejada.

Mais informações sobre a precificação estão disponíveis na documentação da [Classe de armazenamento do {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/help/billing.html#ibm-cos-pricing).

### Classes de Armazenamento de Amostra

**Padrão**
Esse serviço é para dados não estruturados que requerem acesso frequente, como o DevOps, colaboração e repositórios de conteúdo de ação.

**Área segura**
Esse serviço é para cargas de trabalho com dados acessados com pouca frequência, como cargas de trabalho de backup, de archive e de conformidade.

**Área segura fria**
Essa opção de implementação é ideal para requisitos de acesso mínimos, conformidade de registros históricos e backup de longo prazo.

**Flex** Implementação para variar os requisitos de acesso a dados e proteger seu orçamento de flutuações de custo inesperadas.
As classes de armazenamento são definidas no nível de depósito. Simplesmente crie um novo depósito que esteja configurado para a classe de armazenamento desejada.
