---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-05"

keywords: liveness probe swift, readiness probe swift, health swift, healthcheck swift, swift app status, kubernetes endpoint swift, health endpoint swift, swift health check

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Usando uma verificação de funcionamento no app Swift
{: #healthcheck}

As verificações de funcionamento fornecem um mecanismo simples para determinar se um aplicativo do lado do servidor está se comportando corretamente. Ambientes em nuvem, como [Kubernetes](https://www.ibm.com/cloud/container-service){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") e [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") podem ser configurados para pesquisar terminais de funcionamento periodicamente para determinar se uma instância de seu serviço está pronta para aceitar o tráfego.
{: shortdesc}

## Visão geral da verificação de
{: #overview}

As verificações de funcionamento fornecem um mecanismo simples para determinar se um aplicativo do lado do servidor está se comportando corretamente. Elas geralmente são consumidas por meio de HTTP e usam códigos de retorno padrão para indicar o status de UP ou DOWN. O valor de retorno de uma verificação de funcionamento é variável, mas uma resposta JSON mínima, como `{"status": "UP"}`, é típica.

O Kubernetes tem uma noção matizada do funcionamento do processo. Ele define duas análises:

- Uma análise de _**prontidão**_ é usada para indicar se o processo pode manipular solicitações (é roteável).

  O Kubernetes não roteia trabalho para um contêiner com uma análise de prontidão com falha. Uma análise de prontidão poderá falhar se um serviço não terminar de inicializar ou se estiver de outra forma ocupado, sobrecarregado ou incapaz de processar solicitações.

- Uma análise de _**atividade**_ é usada para indicar se o processo deve ser reiniciado.

  O Kubernetes para e reinicia um contêiner com uma análise de atividade com falha. Use as análises de vivacidade para limpar processos em um estado irrecuperável, por exemplo, se a memória estiver esgotada ou se o contêiner não foi parado corretamente após a falha de um processo interno.

Como uma nota para comparação, o Cloud Foundry usa um terminal de funcionamento. Se essa verificação falhar, o processo será reiniciado, mas se ela for bem-sucedida, as solicitações serão roteadas para ele. Neste ambiente, o terminal é minimamente bem-sucedido quando o processo está ativo. Um tempo de espera inicial é configurado para adiar a verificação de funcionamento até que o serviço termine a inicialização para evitar ciclos de reinicialização.

A seguinte tabela fornece diretrizes sobre as respostas que os terminais de prontidão, vivacidade e funcionamento singular devem fornecer.

| Estado    | Prontidão                   | Atividade                   | Funcionamento                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | Não OK causa nenhum carregamento       | Não OK causa reinicialização      | Não OK causa reinicialização     |
| Iniciando | 503 - Indisponível           | 200 - OK                   | Usar atraso para evitar teste   |
| Ativado       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| Parando | 503 - Indisponível           | 200 - OK                   | 503 - Indisponível         |
| Inativo     | 503 - Indisponível           | 503 - Indisponível          | 503 - Indisponível         |
| Com erro  | 500 - Erro do servidor          | 500 - Erro do servidor         | 500 - Erro do servidor        |
{: caption="Tabela 1. Códigos de status de HTTP." caption-side="bottom"}

## Incluindo uma verificação de funcionamento em um app Swift existente
{: #existing-app}

A biblioteca de [Funcionamento](https://github.com/IBM-Swift/Health){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") torna mais fácil incluir uma verificação de funcionamento em seu aplicativo do Swift. As verificações de funcionamento são extensíveis. Para obter mais informações sobre o [armazenamento em cache](https://github.com/IBM-Swift/Health#caching){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para evitar ataques do DoS ou incluir [verificações customizadas](https://github.com/IBM-Swift/Health#implementing-a-health-check){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"), consulte a biblioteca de [Funcionamento](https://github.com/IBM-Swift/Health){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

Para incluir a biblioteca de funcionamento em um app Swift existente, consulte as etapas a seguir:

1. Especifique-o na seção *dependências:* do arquivo `Package.swift` e inclua-o em todos os destinos apropriados:

    ```swift
    .package (url: "https: //github.com/IBM-Swift/Health.git ", .from:" 1.0.0 "),
    ```
    {: codeblock}

2. Inclua o seguinte código de inicialização no aplicativo:

    ```swift
    importar Saúde

    = let Health = Health ()
    ```
    {: codeblock}

3. Inclua a definição de rota para definir o terminal de verificação de funcionamento:

    ```swift
    router.get("/health") { request, response, next in
        /* let status = health.status.toDictionary() */
        let status = health.status.toSimpleDictionary()
        if health.status.state == .UP {
            try response.send(json: status).end()
  } else {
            try response.status(.serviceUnavailable).send(json: status).end()
        }
    }
    ```
    {: codeblock}

4. Verifique o status do app com um navegador, acessando o terminal `/health`. O código retorna uma carga útil `{"status": "UP"}`, conforme definido pelo dicionário simples.

## Verificando o funcionamento de um app do kit do iniciador do Swift do lado do servidor
{: #healthcheck-starterkit}

Quando você gerar um app do Swift baseado em Kitura usando um kit do iniciador, um terminal de verificação de funcionamento básico, `/health`, será incluído por padrão. O terminal usa o protocolo Codable disponível no Swift 4, conforme suportado pela biblioteca de [Funcionamento](https://github.com/IBM-Swift/Health){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

O código de inicialização básico, tal como a inicialização do objeto Health, ocorre em `Sources/Application.swift`. O próprio terminal de verificação de funcionamento é fornecido pelo arquivo `/Sources/Application/Routes/HealthRoutes.swift` e usa o código a seguir:

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

O exemplo usa o dicionário padrão, que produz uma carga útil, como:
```
{"status":"UP","details":[],"timestamp":"2018-07-31T17:41:16+0000"}
```

quando você acessa o terminal `/health`.

## Recomendações para as análises de prontidão e vivacidade
{: #recommend-probes}

As análises de prontidão deverão incluir a viabilidade de conexões com serviços de recebimento de dados em seu resultado se existir um fallback não aceitável para quando o serviço de recebimento de dados estiver indisponível. Isso não significa chamar a verificação de funcionamento que é fornecida pelo serviço de recebimento de dados diretamente, pois a infraestrutura verifica isso para você. Em vez disso, considere verificar o funcionamento das referências existentes que seu aplicativo tem para os serviços de recebimento de dados: essa pode ser uma conexão JMS com o WebSphere MQ ou um consumidor ou produtor Kafka inicializado. Se você verificar a viabilidade de referências internas para os serviços de recebimento de dados, armazene em cache o resultado para minimizar o impacto que a verificação de funcionamento tem no aplicativo.

Uma análise de vivacidade, por contraste, pode ser deliberada sobre o que é verificado, pois uma falha resulta em término imediato do processo. Um terminal HTTP simples que sempre retorna `{"status": "UP"}` com código de status `200` é uma opção razoável.

### Inclua suporte para prontidão e vivacidade do Kubernetes em um app Swift
{: #kube-readiness-swift}

Para implementações alternativas, como usar **Codable** ou o dicionário padrão, consulte [Exemplos de biblioteca de funcionamento](https://github.com/IBM-Swift/Health#usage){: new_window}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"). Algumas dessas implementações simplificam a criação de verificações de funcionamento extensíveis com suporte para verificações de armazenamento em cache que são executadas com relação aos serviços auxiliares. Nesse cenário, você gostaria de separar o teste de vivacidade simples da verificação de prontidão detalhada e mais robusta.

## Configurando as análises de prontidão e de vivacidade no Kubernetes
{: #config-kube-readiness}

Declare as análises de vivacidade e de prontidão juntamente com a implementação do Kubernetes. As duas análises usam os mesmos parâmetros de configuração:

* O kubelet aguarda por `initialDelaySeconds` antes da primeira análise.

* O kubelet analisa o serviço a cada `periodSeconds` segundos. O padrão é 1.

* A análise atinge o tempo limite após `timeoutSeconds` segundos. O valor padrão e o mínimo é 1.

* A análise será bem-sucedida se obtiver êxito `successThreshold` vezes após uma falha. O valor padrão e o mínimo é 1. O valor deve ser 1 para as análises de vivacidade.

* Quando um pod inicia e a análise falha, o Kubernetes tenta `failureThreshold` vezes para reiniciar o pod e, em seguida, desiste. O valor mínimo é 1 e o valor padrão é 3.
    - Para uma análise de vivacidade, "desistir" significa reiniciar o pod.
    - Para uma análise de prontidão, "desistir" significa marcar o pod como `Unready`.

Para evitar os ciclos de reinicialização, configure `livenessProbe.initialDelaySeconds` para ser mais longo, com segurança, que a inicialização do serviço. Em seguida, é possível usar um valor mais curto para `readinessProbe.initialDelaySeconds` para rotear as solicitações para o serviço assim que ele estiver pronto.

Consulte o seguinte exemplo de `yaml`:
```yaml
spec:
  containers:
  - name: ...
    image: ...
    readinessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 120
      timeoutSeconds: 5
    livenessProbe:
      httpGet:
        path: /liveness
        port: 8080
      initialDelaySeconds: 130
      timeoutSeconds: 10
      failureThreshold: 10
```

Para obter mais informações, consulte como [Configurar análises de vida e disponibilidade](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").
