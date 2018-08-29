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

# Usando uma verificação de funcionamento no app Swift
{: #healthcheck}

As verificações de funcionamento fornecem um mecanismo simples para determinar se um aplicativo do lado do servidor está se comportando corretamente ou não. Muitos ambientes de implementação, como o [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) e o [Kubernetes](https://www.ibm.com/cloud/container-service), podem ser configurados para pesquisar os terminais de funcionamento periodicamente para determinar se uma instância de seu serviço está pronta ou não para aceitar tráfego.

As verificações de funcionamento geralmente são consumidas por meio de HTTP e usam códigos de retorno padrão para indicar o status `UP` ou `DOWN`. Os exemplos incluem o retorno de `200` para `UP` e `5xx` para `DOWN`. Para ser específico, `503` é usado quando o aplicativo não pode manipular solicitações ou ainda não foi iniciado (o serviço está indisponível) e `500` é usado quando o servidor encontra uma condição de erro. O valor de retorno de uma verificação de funcionamento é variável, mas uma resposta JSON mínima, como `{“status”: “UP”}` fornece consistência.

O Cloud Foundry usa um terminal de funcionamento para indicar se uma instância de serviço pode ou não manipular solicitações. No Kubernetes, um terminal de verificação de funcionamento é equivalente a um terminal `readiness`. Seu aplicativo deve definir esse terminal para ajudar a determinar as decisões de roteamento automáticas. O sucesso ou a falha desse terminal pode incluir consideração para os serviços de recebimento de dados necessários no caso de não haver fallback aceitável. Se você realmente verifica os serviços de recebimento de dados, o armazenamento em cache do resultado é, às vezes, útil, para minimizar o carregamento no sistema geral devido a verificações de funcionamento.

O Kubernetes define um terminal de [atividade](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) extra, que permite que o aplicativo indique se o processo deve ou não ser reiniciado. Um subconjunto de considerações aplica-se aqui: uma verificação de `atividade` poderá falhar, se um determinado limite de utilização de memória for atingido, por exemplo. Se o seu app estiver em execução no Kubernetes, considere incluir um terminal `liviness` para assegurar que seu processo seja reiniciado quando necessário.

## Incluindo a verificação de funcionamento em um app Swift existente
{: #add-healthcheck-existing}

A biblioteca [Health](https://github.com/IBM-Swift/Health) facilita a inclusão de uma verificação de funcionamento no aplicativo Swift. As verificações de funcionamento são extensíveis. Para obter mais informações sobre o [armazenamento em cache](https://github.com/IBM-Swift/Health#caching) para evitar ataques DoS ou a inclusão de [verificações customizadas](https://github.com/IBM-Swift/Health#implementing-a-health-check), consulte a biblioteca [Health](https://github.com/IBM-Swift/Health).

Para incluir a biblioteca Health em um app Swift existente, especifique-a na seção *dependências:* de seu arquivo `Package.swift`, certificando-se de incluí-la em todos os destinos em que seja usada:
```swift
  .package (url: "https: //github.com/IBM-Swift/Health.git ", .from:" 1.0.0 "),
```
{: codeblock}

Em seguida, inclua o código de inicialização a seguir em seu aplicativo:
```swift
importar Saúde

= let Health = Health ()
```
{: codeblock}

Em seguida, inclua a definição de rota para definir o terminal de verificação de funcionamento:
```
router.get("/health") { request, response, next in
  // let status = health.status.toDictionary()
  let status = health.status.toSimpleDictionary()
  if health.status.state == .UP {
    try response.send(json: status).end()
  } else {
    try response.status (.serviceUnavailable) .send (json: status) .end ()
  }
}
```
{: codeblock}

Verifique o status do app com um navegador, acessando o terminal `/health`. O código retorna uma carga útil `{"status": "UP"}`, conforme definido pelo dicionário simples.

Para implementações alternativas, como o uso de **Codable** ou do dicionário padrão, consulte os [Exemplos de bibliotecas Health](https://github.com/IBM-Swift/Health#usage).

## Acessando a verificação de funcionamento por meio de apps Swift Starter Kit do lado do servidor
{: #healthcheck-starterkit}

Ao gerar um app Swift baseado no Kitura usando um Starter Kit, um terminal de verificação de funcionamento básico, `/health`, é incluído por padrão. O terminal alavanca o protocolo Codable disponível no Swift 4, conforme suportado pela biblioteca [Health](https://github.com/IBM-Swift/Health).

O código de inicialização básico, como a inicialização do objeto Health, ocorre em `Sources/Application.swift`, enquanto o terminal de verificação de funcionamento é fornecido pelo arquivo `/Sources/Application/Routes/HealthRoutes.swift`, que contém o código a seguir:
```swift
import LoggerAPI import Health import KituraContracts

func initializeHealthRoutes (app: App) {

    app.router.get ("/health") { -> Void) -> Void in
        if health.status.state == .UP {
            respondWith(health.status, nil)
        } else {
            respondWith(nil, RequestError(.serviceUnavailable, body: health.status))
        }
    }

}
```
{: codeblock}

O exemplo usa o dicionário padrão, que produz uma carga útil, como `{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}` quando você acessa o terminal `/health`.
