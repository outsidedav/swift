---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip} 

# Edificação com segurança avançada
{: #security}

Os desenvolvedores de Swift podem usar facilmente serviços de segurança avançados que o {{site.data.keyword.cloud}} oferece para proteger suas chaves e dados em repouso, em uso ou em trânsito no nível de segurança mais alto da indústria.
{: shortdesc}

Como uma abordagem fácil, é possível usar diretamente o {{site.data.keyword.cloud_notm}} HyperSecure Platform Services para todos os serviços de segurança avançada no {{site.data.keyword.cloud}}. Para obter mais informações, consulte [Introdução ao {{site.data.keyword.cloud_notm}} HyperSecure Platform Services](/docs/services/hypersecure-platform/index.html){:new_window}.

## Usando o  {{site.data.keyword.cloud_notm}}  {{site.data.keyword.hscrypto}}
{: #security-swift}

O {{site.data.keyword.hscrypto}} é um serviço experimental que oferece criptografia em suas chaves e dados com o nível de segurança mais alto da indústria. Ele traz a segurança e a integridade da IBM Z para a nuvem. A mesma tecnologia criptográfica dos bancos e dos serviços financeiros agora é oferecida aos usuários de nuvem por meio do {{site.data.keyword.cloud_notm}}.

O {{site.data.keyword.hscrypto}} oferece módulos de segurança de hardware endereçáveis de rede (HSMs) para fornecer criptografia segura e protegida por meio de interfaces de programação de aplicativos (APIs) PKCS#11. É possível acessar o nível mais alto possível de segurança para o hardware de criptografia, ou seja, FIPS 140-2 Nível 4. O {{site.data.keyword.hscrypto}} também usa a solução IBM Advanced Crypto Service Provider (ACSP) que permite o acesso remoto aos coprocessadores criptográficos da IBM.

O {{site.data.keyword.hscrypto}} também é o keystore para o serviço {{site.data.keyword.keymanagementserviceshort}}.

Para obter mais informações sobre o {{site.data.keyword.hscrypto}}, consulte [Introdução ao {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/services/hs-crypto/index.html){:new_window}.

## Usando o {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}
{: #swift-key-management}

O {{site.data.keyword.keymanagementserviceshort}} ajuda a criar chaves criptografadas para aplicativos entre os serviços do {{site.data.keyword.cloud_notm}}. Conforme você gerencia o ciclo de vida de suas chaves, é possível se beneficiar de saber que as suas chaves estão protegidas pelos módulos de segurança de hardware (HSMs) baseados em nuvem que protegem contra roubo de informações. Junto ao {{site.data.keyword.hscrypto}}, suas chaves são protegidas no nível de segurança mais alto do certificado FIPS 140-2 Nível 4.

Para obter mais informações sobre o {{site.data.keyword.keymanagementserviceshort}}, consulte [Introdução ao {{site.data.keyword.keymanagementserviceshort}}](/docs/services/keymgmt/index.html){:new_window}.

## Usando o  {{site.data.keyword.cloud_notm}}  Hyper Protect DBaaS
{: #hypersecure-dbaas}

O {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS é um serviço experimental {{site.data.keyword.cloud_notm}} que cria bancos de dados seguros sob demanda. Ele oferece uma plataforma extensível para fornecer e gerenciar de maneira rápida e fácil o banco de dados MongoDB.

Com esse serviço, é possível criar clusters de banco de dados no {{site.data.keyword.cloud_notm}}. Cada cluster de banco de dados que você cria tem uma instância de banco de dados principal e duas instâncias de banco de dados secundárias que servem como réplicas para o primário. A lógica do {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS cria e acessa bancos de dados MongoDB nos clusters de banco de dados.

### Protegendo dados e privacidade
{: #swift-securing-data}

A {{site.data.keyword.IBM_notm}} hospeda seus bancos de dados em um ambiente altamente disponível e seguro:
 * Os dados são criptografados em repouso e em andamento.
 * O hardware do sistema, a configuração do sistema e a configuração do banco de dados asseguram alta disponibilidade.
 * As tecnologias subjacentes evitam que a {{site.data.keyword.IBM_notm}} ou um terceiro possa acessar seus dados.

### Incluindo a lógica do {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS em seu aplicativo
{: #swift-hyperprotect}

Para usar um banco de dados MongoDB em seu aplicativo, consulte
[Introdução ao uso de um banco de dados](/docs/hypersecure_dbaas/database-cluster.html).  

### Aprendendo mais sobre o  {{site.data.keyword.cloud_notm}}  Hyper Protect DBaaS
{: #swift-learnmore}

Para saber mais sobre o {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS, consulte [Introdução ao IBM Cloud Hyper Protect DBaaS](/docs/services/hyper-protect-dbaas/index.html).

## Usando o {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}}
{: #swift-hscontainers}

O {{site.data.keyword.hscontainers}} fornece ferramentas poderosas, combinando s tecnologias Docker e Kubernetes, uma experiência intuitiva do usuário e a segurança e o isolamento integrados para automatizar a implementação, a operação, o ajuste de escala e o monitoramento de apps conteinerizados em um cluster de hosts de cálculo.

Os {{site.data.keyword.hscontainers}} estão disponíveis apenas para usuários patrocinados. Se você espera suporte de segurança dedicado, registre-se como um usuário patrocinado com o [IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml) para implementar seu aplicativo no cluster do {{site.data.keyword.hscontainers}}.
{: tip}

Antes de se tornar um usuário patrocinado, é possível usar o {{site.data.keyword.containershort_notm}} para proteger seu aplicativo. Para obter mais informações, consulte
[Introdução ao {{site.data.keyword.containershort_notm}}](/docs/containers/container_index.html).
