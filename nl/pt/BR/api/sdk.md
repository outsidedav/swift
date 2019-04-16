---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: swift sdk plug-in, sdk generator swift, generated sdk swift, devops pipeline swift, open api swift, sdkgen swift, ibmcloud sdk swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Incluindo serviços de backend em seu app com um SDK gerado
{: #sdk-cli}

O plug-in do {{site.data.keyword.IBM}} SDK Generator pode ser instalado na CLI do [{{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

Com o plug-in do {{site.data.keyword.IBM_notm}} SDK Generator, é possível integrar os serviços de back-end ao app usando um SDK gerado. Quando ocorrer uma mudança em uma API de REST, será possível gerar novamente o SDK e substituir o antigo para fazer upgrade facilmente do SDK. É possível, então, incluir a CLI em um pipeline do DevOps e assegurar que o SDK seja sempre consistente com a especificação de API toda vez que o aplicativo for construído.

A definição da API de REST deve ser válida e hospedada em um end point do servidor ativo ou em um arquivo local em seu sistema.

## Antes de começar
{: #prereqs-sdk-cli}

Assegure-se de que você tenha os pré-requisitos a seguir:

* Uma conta do [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").
* Uma definição de API válida que esteja em conformidade com a especificação [Open API ](https://www.openapis.org/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Instalando o plug-in SDK
{: #install-sdk-cli}

1. [ Instale a  {{site.data.keyword.cloud_notm}}  CLI ](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

2. Instale o plug-in do SDK:
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

É possível instalar o [{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#install_plug-in) que inclui a CLI de base do {{site.data.keyword.cloud_notm}} e também inclui o plug-in `sdk-gen` juntamente com outras ferramentas locais úteis.
{: tip}

## Gerando o SDK
{: #commands-sdk-cli}

Gerar um SDK inserindo:
```
ibmcloud sdk generate [ argumentos ...] [ opções de comando ]
```
{: codeblock}

### Argumentos
{: #gen-args-sdk-cli}

* **APP_NAME** - O nome do app Cloud Foundry no espaço atual.
* **OPENAPI_DOC_LOCATION**: uma URL ou um caminho de arquivo relativo para a definição de API de REST bruta JSON ou yaml.
* **GENERATED_SDK_NAME** (opcional) - O nome do SDK gerado.

### Opções
{: #gen-options-sdk-cli}

** Plataforma **  (obrigatório):
  * `--ios` - Gerar um SDK do iOS Swift.
  * `--swift` - Gerar um SDK do servidor Swift.
  * ` -- js ` -Gerar um SDK JavaScript.

** Local **  (obrigatório):

Especifica o tipo para o argumento `OPENAPI_DOC_LOCATION`.

  * ` -r ` -Uma URL remota.
  * ` -f ` -Um nome de arquivo.
  * ` -a ` -O aplicativo que é executado no  {{site.data.keyword.cloud_notm}}
  * ` -l ` -A URL do host local.

** Opcional **:
  * `--output "YOUR_RELATIVE_PATH"` - Coloca o SDK gerado no diretório especificado por `YOUR_RELATIVE_PATH` (Os SDKs existentes serão sobrescritos, se presentes).
  * `--unzip`: extrai o SDK gerado (sobrescreve os dados existentes se os artefatos de SDK estão presentes).

### Uso
{: #gen-usage-sdk-cli}

Para gerar um SDK de um app Cloud Foundry que está em execução no {{site.data.keyword.cloud_notm}}, é possível usar o nome do app como um parâmetro para a CLI. O comando a seguir usa o nome do app como o `SDK_Name`.

```
Ibmcloud sdk generate [ APP_NAME ] [ LOCATION ] [ PLATFORM ]
```
{: codeblock}

Para gerar um SDK de uma URL para um arquivo de definição de Open API ou um JSON local ou arquivo yaml, use o comando a seguir.

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Validando as Definições de Open API
{: #validating-sdk-cli}

Execute o comando a seguir: `ibmcloud sdk validate [argument]`

### Argumentos
{: #val-args-sdk-cli}

* `APP_NAME` - O nome do app Cloud Foundry no espaço atual.
* `OPENAPI_DOC_LOCATION`: uma URL ou um caminho de arquivo relativo para a definição de API de REST bruta JSON ou yaml.

### Uso
{: #val-usage-sdk-cli}

Para validar uma especificação de API do app Cloud Foundry que está em execução no {{site.data.keyword.cloud_notm}}, é possível usar o nome do app como um parâmetro para a CLI.
```
Ibmcloud sdk validate [ APP_NAME ] [ LOCATION ]
```
{: codeblock}

Para validar um SDK por meio da URL para um documento de especificação de API ou um arquivo JSON ou yaml local, use o comando a seguir:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

