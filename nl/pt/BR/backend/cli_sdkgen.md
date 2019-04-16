---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-28"

keywords: generated sdk swift, devops pipeline swift, sdk plug-in swift, open api swift, sdkgen swift, ibmcloud sdk swift, swift backend service

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Integrando serviços de backend a seu app com um SDK gerado
{: #sdkgen-cli}

O plug-in do {{site.data.keyword.IBM}} SDK Generator pode ser instalado no [{{site.data.keyword.cloud}} CLI ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli){: new_window}.

Esse plug-in {{site.data.keyword.IBM_notm}} SDK Generator integra os serviços de backend a seu app com um SDK gerado. Quando ocorrer uma mudança em uma API de REST, será possível gerar novamente o SDK e substituir o antigo para facilmente fazer o upgrade do SDK. Também é possível integrar a CLI a um pipeline do DevOps e assegurar-se de que o SDK esteja
sempre consistente com a especificação de API toda vez que o app é construído.

A definição da API de REST deve ser válida e hospedada em um end point do servidor ativo ou em um arquivo local em seu sistema.

## Antes de começar
{: #prereqs-sdkgen}

Assegure-se de que você tenha:

* Uma conta do [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")
* Uma definição de API válida que é adequada à especificação de [Open API ](https://www.openapis.org/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")

## Instalando o plug-in SDK
{: #install-sdkgen}

1. [ Instale a  {{site.data.keyword.cloud_notm}}  CLI ](/docs/cli?topic=cloud-cli-install-ibmcloud-cli#install-ibmcloud-cli).

2. Instale o plug-in `sdk-gen`:
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

É possível instalar o [{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#install_plug-in) que inclui a CLI de base do {{site.data.keyword.cloud_notm}} e também inclui o plug-in `sdk-gen` juntamente com outras ferramentas locais úteis.
{: tip}

## Gerando o SDK
{: #commands-sdkgen}

Gere um SDK inserindo: `ibmcloud sdk generate [arguments...] [command options]`

### Argumentos
{: #gen-args-sdkgen}

* `APP_NAME` - o nome do app Cloud Foundry em seu espaço atual
* `OPENAPI_DOC_LOCATION` - uma URL ou um caminho relativo de arquivo para a definição de API de REST bruta JSON ou yaml
* `GENERATED_SDK_NAME` (opcional) - o nome do SDK gerado

### Opções
{: #gen-options-sdkgen}

* ` PLATFORM `  (necessário)
   * `--ios` - gerar um SDK do iOS Swift
   * `--swift` - gerar um SDK do servidor Swift
   * ` -- js ` -gerar um SDK JavaScript
* `LOCATION` (obrigatório) - especifica o tipo para `OPENAPI_DOC_LOCATION`
   * ` -r ` -URL remota
   * `-f` - arquivo
   * ` -a ` -app executado no  {{site.data.keyword.cloud_notm}}
   * ` -l ` -URL do host local
* `--output "YOUR_RELATIVE_PATH"` (opcional): coloca o SDK gerado no diretório especificado por `YOUR_RELATIVE_PATH` (os SDKs existentes são sobrescritos).
* `--unzip` (opcional): extrai o SDK gerado (os SDKs existentes são sobrescritos).

### Uso
{: #gen-usage-sdkgen}

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
{: #validating-sdkgen-sdkgen}

Execute o comando a seguir:
```
ibmcloud sdk validate [ argument ]
```
{: codeblock}

### Argumentos
{: #val-args-sdkgen}

* `APP_NAME` - o nome do app Cloud Foundry em seu espaço atual
* `OPENAPI_DOC_LOCATION` - uma URL ou um caminho relativo de arquivo para a definição de API de REST bruta JSON ou yaml

### Uso
{: #val-usage-sdkgen}

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
