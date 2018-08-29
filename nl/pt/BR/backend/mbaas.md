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
{:note:.deprecated}

# MBaaS versus componentes de backend
{: #mbaas}

Seu aplicativo pode usar recursos externos por meio de uma de duas abordagens:
* Uso direto do aplicativo para seus serviços, que geralmente são hospedados como parte de uma plataforma Backend móvel como serviço (MBaaS).
* Uso por meio de um componente Backend For Frontend (BFF) hospedado, que pode fornecer composição e controle sobre os serviços e pode incluir lógica de backend para o aplicativo.

Geralmente, a inclusão de recursos externos diretamente do aplicativo móvel por meio de uma abordagem do estilo MBaaS é mais direta. Menos componentes e infraestrutura são usados para incluir o primeiro conjunto de serviços. No entanto, esse método não apresenta a flexibilidade final e o escopo possível por meio da implementação de um BFF.

Uma das vantagens da plataforma {{site.data.keyword.cloud_notm}} é que você não precisa decidir à frente. Todos os serviços fornecidos podem ser utilizados diretamente do dispositivo móvel usando SDKs fornecidos baseados no Swift. A plataforma {{site.data.keyword.cloud_notm}} também suporta a gravação de sua lógica de backend no Swift, seja por meio de um modelo sem servidor com o {{site.data.keyword.openwhisk_short}} ou por meio do Swift do lado do servidor com estruturas como o Kitura. Por causa dessa funcionalidade, quando você deseja incluir lógica de backend por meio de um modelo BFF, é possível fazer isso no Swift e optar por migrar o uso dos SDKs do Swift do aplicativo para o BFF. Como resultado, é possível mover aos poucos de uma abordagem do estilo MBaaS para uma abordagem do BFF com sobrecarga mínima por causa do suporte de ponta a ponta do Swift.
