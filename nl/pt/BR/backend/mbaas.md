---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Recursos do back-end móvel como serviço
{: #mbaas}

O aplicativo pode usar recursos externos por meio de uma de duas abordagens:
* Uso direto do aplicativo para seus serviços, que geralmente são hospedados como parte de uma plataforma Backend móvel como serviço (MBaaS).
* Uso por meio de um componente Backend For Frontend (BFF) hospedado, que pode fornecer composição e controle sobre os serviços e pode incluir lógica de backend para o aplicativo.

Geralmente, incluir recursos externos diretamente do aplicativo móvel por meio de uma abordagem de estilo MBaaS é mais direto. Menos itens e infraestrutura são necessários para incluir o primeiro conjunto de serviços. No entanto, esse método não apresenta a flexibilidade final e o escopo possível por meio da implementação de um BFF.

Uma das vantagens da plataforma {{site.data.keyword.cloud_notm}} é que você não precisa decidir à frente. Todos os serviços fornecidos podem ser usados diretamente por meio do dispositivo móvel usando os SDKs baseados em Swift fornecidos. A plataforma {{site.data.keyword.cloud_notm}} também suporta a gravação de sua lógica de backend no Swift, seja por meio de um modelo sem servidor com o {{site.data.keyword.openwhisk_short}} ou por meio do Swift do lado do servidor com estruturas como o Kitura. Por causa dessa funcionalidade, quando você deseja incluir lógica de backend por meio de um modelo BFF, é possível fazer isso no Swift e optar por migrar o uso dos SDKs do Swift do aplicativo para o BFF. Como resultado, é possível mover-se incrementalmente de uma abordagem de estilo MBaaS para uma abordagem BFF com o mínimo de esforço extra.
