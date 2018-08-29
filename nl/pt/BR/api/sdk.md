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

# Incluindo serviços de backend em seu app com um SDK gerado
{: #sdk-cli}

O plug-in {{site.data.keyword.IBM}} SDK Generator pode ser instalado na [CLI do {{site.data.keyword.cloud_notm}}](/docs/cli/reference/bluemix_cli/get_started.html).

Com o plug-in {{site.data.keyword.IBM_notm}} SDK Generator, é possível integrar facilmente os serviços de backend a seu app usando um SDK gerado. Quando ocorre uma mudança em uma API de REST, é possível gerar novamente o SDK e substituir o antigo por um upgrade do SDK sem interrupção. Também é possível integrar a CLI a um pipeline devops e assegurar que o SDK seja sempre consistente com a especificação de API sempre que o app é construído.

A definição da API de REST deve ser válida e hospedada em um end point do servidor ativo ou em um arquivo local em seu sistema.

## Antes de começar
{: #prereqs}

Assegure-se de que você tenha os pré-requisitos a seguir:

* Uma conta do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://bluemix.net){: new_window}.
* Uma definição de API válida que esteja em conformidade com a especificação [Open API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.openapis.org/){: new_window}.

## Instalando o plug-in SDK
{: #installation}

1. [ Instale a  {{site.data.keyword.cloud_notm}}  CLI ](/docs/cli/reference/bluemix_cli/get_started.html).

2. [ Instale o plug-in SDK ](/docs/cli/sdk/index.html).
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## Gerando o SDK
{: #commands}

Gerar um SDK inserindo:
```
ibmcloud sdk generate [ argumentos ...] [ opções de comando ]
```
{: codeblock}

### Argumentos
{: #gen-args}

* **APP_NAME** - O nome do app Cloud Foundry no espaço atual.
* **OPENAPI_DOC_LOCATION** - Uma URL ou um caminho de arquivo relativo para a definição bruta de API de REST JSON ou Yaml.
* **GENERATED_SDK_NAME** (opcional) - O nome do SDK gerado.

### Opções
{: #gen-options}

** Plataforma **  (obrigatório):
  * `--ios` - Gerar um SDK do iOS Swift.
  * `--swift` - Gerar um SDK do servidor Swift.
  * ` -- js ` -Gerar um SDK JavaScript.

** Local **  (obrigatório):

Especifica o tipo para o argumento `OPENAPI_DOC_LOCATION`.

  * ` -r ` -Uma URL remota.
  * ` -f ` -Um nome de arquivo.
  * ` -a ` -O aplicativo que é executado no  {{site.data.keyword.coud_notm}}
  * ` -l ` -A URL do host local.

** Opcional **:
  * `--output "YOUR_RELATIVE_PATH"` - Coloca o SDK gerado no diretório especificado por `YOUR_RELATIVE_PATH` (Os SDKs existentes serão sobrescritos, se presentes).
  * `--unzip` - Extrai o SDK gerado (sobrescreverá dados se artefatos SDK existentes estiverem presentes).

### Uso
{: #gen-usage}

Para gerar um SDK de um app Cloud Foundry que está em execução no {{site.data.keyword.cloud_notm}}, é possível usar o nome do app como um parâmetro para a CLI. O comando a seguir usa o nome do app como o `SDK_Name`.

```
Ibmcloud sdk generate [ APP_NAME ] [ LOCATION ] [ PLATFORM ]
```
{: codeblock}

Para gerar um SDK por meio de uma URL para um arquivo de definição de API Aberto ou um arquivo de JSON ou Yaml local, use o comando a seguir.

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Validando as Definições de Open API
{: #validating}

Execute o comando a seguir: `ibmcloud sdk validate [argument]`

### Argumentos
{: #val-args}

* `APP_NAME` - o nome do app Cloud Foundry em seu espaço atual
* `OPENAPI_DOC_LOCATION` - uma URL ou um caminho de arquivo relativo para a definição de JSON ou Yaml da API de REST bruta

### Uso
{: #val-usage}

Para validar uma especificação de API do app Cloud Foundry que está em execução no {{site.data.keyword.cloud_notm}}, é possível usar o nome do app como um parâmetro para a CLI.
```
Ibmcloud sdk validate [ APP_NAME ] [ LOCATION ]
```
{: codeblock}

Para validar um SDK da URL para um documento de especificação de API ou um arquivo de JSON ou Yaml local, use o comando a seguir:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

