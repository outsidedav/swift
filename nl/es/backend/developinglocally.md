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

# Desarrollo de forma local

Al desarrollar localmente, puede crear, ejecutar y probar fácilmente las apps de Swift. Utilice el {{site.data.keyword.dev_cli_short}} para realizar estas acciones mediante los mandatos estándares. 

Puede utilizar el {{site.data.keyword.dev_cli_short}} para gestionar las aplicaciones del lado del servidor con más de doce mandatos. Obtenga más información sobre los distintos mandatos en los [mandatos de IBM Cloud Developer Tools CLI `ibmcloud dev`](/docs/cli/idt/commands.html).

## Antes de empezar

Para desarrollar localmente, debe instalar el {{site.data.keyword.dev_cli_notm}}. Ejecute el mandato siguiente para ejecutar el script de instalación:
```
curl -sL https://ibm.biz/idt-installer | bash
```
{:codeblock}

Consulte [Configuración de la CLI de IBM Cloud Developer Tools](/docs/cli/idt/setting_up_idt.html) para obtener más información sobre la configuración y el uso de la {{site.data.keyword.dev_cli_notm}}.

## Recuperación de las credenciales de servicio

Después de clonar la aplicación desde Git, debe recuperar las credenciales para los servicios que están enlazados a la aplicación, ya que no están almacenados en el repositorio git para la aplicación. La recuperación de las credenciales permite el uso de servicios enlazados. Puede descargar fácilmente las credenciales ejecutando el mandato siguiente en la raíz del directorio de la aplicación:
```
ibmcloud dev get-credentials
```
{:codeblock}

## Creación, ejecución y despliegue de la aplicación

1. **Crear**: Ahora puede crear la aplicación, que es un requisito previo para ejecutar la aplicación.
  Utilice el mandato siguiente en la raíz del directorio de la aplicación para crear la app:
  ```
  ibmcloud dev build
  ```
  {:codeblock}

2. **Ejecutar**: Después de una creación correcta, puede ejecutar la aplicación en un contenedor local con el mandato siguiente:
  ```
  ibmcloud dev run
  ```
  {:codeblock}

  Se visualiza un host y un puerto locales para ver la página de destino de la aplicación si el mandato se ejecuta correctamente.

3. **Desplegar**: Despliegue la aplicación en el {{site.data.keyword.Bluemix_notm}} con el mandato `deploy`:
  ```
  ibmcloud dev deploy
  ```
  {:codeblock}
