---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: develop swift, swift local, service credentials swift, developer tools swift, swift cli, ibmcloud build swift, ibmcloud swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Desarrollo de forma local
{: #develop-locally}

Al desarrollar localmente, puede crear, ejecutar y probar fácilmente las apps de Swift. Utilice el {{site.data.keyword.dev_cli_short}} para realizar estas acciones mediante los mandatos estándares. 

Puede utilizar el {{site.data.keyword.dev_cli_short}} para gestionar las aplicaciones del lado del servidor con más de doce mandatos. Obtenga más información sobre los diversos mandatos en [Mandatos `ibmcloud dev` de la CLI de {{site.data.keyword.dev_cli_notm}}](/docs/cli/idt?topic=cloud-cli-idt-cli#idt-cli).

## Antes de empezar
{: #prereqs-local}

Para desarrollar localmente, debe instalar el {{site.data.keyword.dev_cli_notm}}. Ejecute el mandato siguiente para ejecutar el script de instalación:
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

Consulte [Configuración de la CLI de {{site.data.keyword.dev_cli_notm}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) para obtener más información sobre la configuración y el uso de
{{site.data.keyword.dev_cli_notm}}.

## Recuperación de las credenciales de servicio
{: #credentials-local}

Después de clonar la aplicación desde Git, debe recuperar las credenciales para los servicios que están enlazados a la aplicación, ya que no están almacenados en el repositorio Git para la aplicación. La recuperación de las credenciales permite el uso de servicios enlazados. Puede descargar fácilmente las credenciales ejecutando el mandato siguiente en la raíz del directorio de la aplicación:
```
ibmcloud dev get-credentials
```
{: codeblock}

## Creación, ejecución y despliegue de la aplicación
{: #build-deploy-local}

1. **Crear**: Ahora puede crear la aplicación, que es un requisito previo para ejecutar la aplicación.
  Utilice el mandato siguiente en la raíz del directorio de la aplicación para crear la app:
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Ejecutar**: Después de una creación correcta, puede ejecutar la aplicación en un contenedor local con el mandato siguiente:
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Se visualiza un host y un puerto locales para ver la página de destino de la aplicación si el mandato se ejecuta correctamente.

3. **Desplegar**: Despliegue la aplicación en el {{site.data.keyword.cloud_notm}} con el mandato `deploy`:
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
