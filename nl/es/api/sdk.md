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

# Adición de servicios de fondo a la app con un SDK generado
{: #sdk-cli}

El plugin SDK Generator de {{site.data.keyword.IBM}} se puede instalar en la [CLI de {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

Con el plug-in SDK Generator de {{site.data.keyword.IBM_notm}}, puede integrar los servicios de fondo en la app mediante un SDK generado. Cuando se produzca un cambio en una API REST, puede volver a generar el SDK y sustituir el antiguo para actualizar fácilmente el SDK. Luego puede añadir la CLI a un conducto de DevOps y garantizar que el SDK sea siempre coherente con la especificación de la API cada vez que se cree la app.

La definición de la API REST debe ser válida y estar alojada en un punto final de servidor activo o en un archivo local en el sistema.

## Antes de empezar
{: #prereqs-sdk-cli}

Asegúrese de cumplir los siguientes requisitos previos:

* Una cuenta de [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").
* Una definición de API válida que se ajusta a la [especificación de Open API ](https://www.openapis.org/){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

## Instalación del plugin de SDK
{: #install-sdk-cli}

1. [Instale la CLI de {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

2. Instale el plugin de SDK:
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

Puede instalar
[{{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#install_plug-in), que incluye la CLI base de
{{site.data.keyword.cloud_notm}}, así como el plugin `sdk-gen` junto a otras herramientas útiles locales.
{: tip}

## Generación del SDK
{: #commands-sdk-cli}

Genere un SDK especificando:
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### Argumentos
{: #gen-args-sdk-cli}

* **APP_NAME**: El nombre de la app de Cloud Foundry en el espacio actual.
* **OPENAPI_DOC_LOCATION**: Un URL o una vía de acceso de archivos relativa al archivo JSON o yaml de la definición de la API REST sin formato.
* **GENERATED_SDK_NAME** (opcional): El nombre del SDK generado.

### Opciones
{: #gen-options-sdk-cli}

**Platform** (necesario):
  * `--ios`: Generar un SDK de Swift de iOS.
  * `--swift`: Generar un SDK de servidor de Swift.
  * `--js`: Generar un SDK de JavaScript.

**Location** (necesario):

Especifica el tipo para el argumento `OPENAPI_DOC_LOCATION`.

  * `-r`: Un URL remoto.
  * `-f`: Un nombre de archivo.
  * `-a`: App que se ejecuta en {{site.data.keyword.cloud_notm}}.
  * `-l`: El URL de localhost.

**Opcional**:
  * `--output "YOUR_RELATIVE_PATH"`: Coloca el SDK generado en el directorio especificado por `YOUR_RELATIVE_PATH` (los SDK existentes se sobrescriben, si los hay).
  * `--unzip`: Extrae el SDK generado (los datos existentes se sobrescriben si hay artefactos SDK).

### Uso
{: #gen-usage-sdk-cli}

Para generar un SDK desde una app de Cloud Foundry que se está ejecutando en {{site.data.keyword.cloud_notm}}, puede utilizar el nombre de la app como un parámetro para la CLI. El mandato siguiente utiliza el nombre de la app como el `SDK_Name`.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Para generar un SDK desde un URL a un archivo de definición de Open API o un archivo JSON o yaml local, utilice el siguiente mandato.

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Validación de las definiciones de Open API
{: #validating-sdk-cli}

Ejecute el siguiente mandato: `ibmcloud sdk validate [argument]`

### Argumentos
{: #val-args-sdk-cli}

* `APP_NAME`: El nombre de la app de Cloud Foundry en el espacio actual.
* `OPENAPI_DOC_LOCATION`: Un URL o una vía de acceso de archivos relativa al archivo JSON o yaml de la definición de la API REST sin formato.

### Uso
{: #val-usage-sdk-cli}

Para validar una especificación de API de la app Cloud Foundry que se está ejecutando en {{site.data.keyword.cloud_notm}}, puede utilizar el nombre de la app como un parámetro para la CLI.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Para validar un SDK desde el URL a un documento de especificación de la API o un archivo JSON o yaml local, utilice el siguiente mandato:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

