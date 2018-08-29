---
copyright:
  years: 2017, 2018
lastupdated: "2018-08-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Integrando serviços de backend a seu app com um SDK gerado
{: #sdk-cli}

O plug-in do {{site.data.keyword.IBM}} SDK Generator pode ser instalado no [{{site.data.keyword.Bluemix_notm}} CLI ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/reference/bluemix_cli/index.html){: new_window}.

Esse plug-in {{site.data.keyword.IBM_notm}} SDK Generator integra os serviços de backend a seu app com um SDK gerado. Quando ocorrer uma mudança em uma API de REST, será possível gerar novamente o SDK e substituir o antigo por um upgrade de SDK sem interrupção. Também é possível integrar a CLI em um pipeline devops e assegurar que o SDK seja sempre consistente com a especificação de API toda vez que o app for construído.

A definição da API de REST deve ser válida e hospedada em um end point do servidor ativo ou em um arquivo local em seu sistema.

## Antes de começar
{: #prereqs}

Assegure-se de que você tenha:

* Uma conta do [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://bluemix.net){: new_window}
* Uma definição de API válida que é adequada à especificação de [Open API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.openapis.org/){: new_window}

## Instalando o plug-in SDK
{: #installation}

1. [ Instale a  {{site.data.keyword.Bluemix}}  CLI ](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Instale o plug-in ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in){: new_window}.

  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## Gerando o SDK
{: #commands}

Gere um SDK inserindo: `ibmcloud sdk generate [arguments...] [command options]`

### Argumentos
{: #gen-args}

* `APP_NAME` - o nome do app Cloud Foundry em seu espaço atual
* `OPENAPI_DOC_LOCATION` - uma URL ou um caminho de arquivo relativo para a definição de JSON ou Yaml da API de REST bruta
* `GENERATED_SDK_NAME` (opcional) - o nome do SDK gerado

### Opções
{: #gen-options}

* ` PLATFORM `  (necessário)
   * `--ios` - gerar um SDK do iOS Swift
   * `--swift` - gerar um SDK do servidor Swift
   * ` -- js ` -gerar um SDK JavaScript
* `LOCATION` (obrigatório) - especifica o tipo para `OPENAPI_DOC_LOCATION`
   * ` -r ` -URL remota
   * `-f` - arquivo
   * ` -a ` -app executado no  {{site.data.keyword.Bluemix_notm}}
   * ` -l ` -URL do host local
* `--output "YOUR_RELATIVE_PATH"` (opcional) - coloca o SDK gerado no diretório especificado por `YOUR_RELATIVE_PATH` (Se um SDK existente estiver presente, ele será sobrescrito.)
* `--unzip` (opcional) - extrai o SDK gerado (se artefatos SDK existentes estiverem presentes, eles serão sobrescritos.)

### Uso
{: #gen-usage}

Para gerar um SDK de um app Cloud Foundry que está em execução no {{site.data.keyword.Bluemix_notm}}, é possível usar o nome do app como um parâmetro para a CLI. O comando a seguir usa o nome do app como o `SDK_Name`.

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

Execute o comando a seguir:
```
ibmcloud sdk validate [ argument ]
```
{: codeblock}

### Argumentos
{: #val-args}

* `APP_NAME` - o nome do app Cloud Foundry em seu espaço atual
* `OPENAPI_DOC_LOCATION` - uma URL ou um caminho de arquivo relativo para a definição de JSON ou Yaml da API de REST bruta

### Uso
{: #val-usage}

Para validar uma especificação de API do app Cloud Foundry que está em execução no {{site.data.keyword.Bluemix_notm}}, é possível usar o nome do app como um parâmetro para a CLI.
```
Ibmcloud sdk validate [ APP_NAME ] [ LOCATION ]
```
{: codeblock}

Para validar um SDK da URL para um documento de especificação de API ou um arquivo de JSON ou Yaml local, use o comando a seguir:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
