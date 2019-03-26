---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilización de Object Storage para el contenido estático
{: #object-storage}

Object Storage es un componente fundamental del cálculo en la nube y proporciona potentes características a los desarrolladores de Apple y sus aplicaciones. A diferencia del almacenamiento de información en una jerarquía de archivos (por ejemplo, almacenamiento en bloque o archivo), un almacén de objetos consta únicamente de los archivos y sus metadatos, almacenados en colecciones conocidas como receptáculos. Por definición, estos objetos son inmutables, lo que los convierte en perfectos para datos como imágenes, vídeos y otros documentos estáticos. Para los datos que cambian con frecuencia o que son relacionales, puede utilizar los servicios de base de datos [NoSQL](/docs/swift/data/nosql.html), [Cloudant](/docs/swift/data/cloudant.html) y [SQL](/docs/swift/data/sql.html).

{{site.data.keyword.cos_full_notm}} (COS) es un sistema de almacenamiento que se puede utilizar para almacenar datos no estructurados que son flexibles, rentables y escalables. Se puede acceder a los datos a través de SDK o mediante la interfaz de usuario de IBM. Puede utilizar {{site.data.keyword.cos_full_notm}} para acceder a los datos no estructurados a través de un portal de autoservicio respaldado por las API y los SDK RESTful. 

En función de sus necesidades, puede utilizar {{site.data.keyword.cos_short}} para los servicios siguientes:

* Repositorio de contenido
* Copia de seguridad y recuperación
* Archivado de datos
* Servicios de archivos
* Transferencia de datos

Al crear un receptáculo, debe seleccionar un nivel de resiliencia (varias regiones o regional). En función de su selección, los datos se dispersan y se almacenan en más de una ubicación geográfica.

## API
{: #api-cos}

La API de {{site.data.keyword.cos_full}} es una API basada en REST para leer y escribir objetos. Da soporte a un subconjunto de la API de S3 para facilitar la migración de aplicaciones a {{site.data.keyword.cloud_notm}}. Cualquier SDK de S3 se puede utilizar para usar {{site.data.keyword.cos_full}}. Para obtener más información, consulte la [Referencia de la API de {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/api-reference/about-compatibility-api.html#about-the-ibm-cloud-object-storage-api) completa

## Seguridad
{: #security-cos}

{{site.data.keyword.cos_full_notm}} es muy seguro. Inicialmente, solo el receptáculo y los propietarios de los objetos tienen acceso originalmente al servicio {{site.data.keyword.cos_full_notm}} que crean. También da soporte a la autenticación de usuarios para acceder a los datos. Utilice mecanismos de control de acceso como, por ejemplo, políticas de receptáculos para otorgar permisos de forma selectiva a usuarios y aplicaciones. Puede transferir de forma segura los datos a {{site.data.keyword.cos_short}} a través de puntos finales SSL utilizando el protocolo HTTPS. Si necesita seguridad adicional, puede utilizar la opción Cifrado del lado del servidor (SSE) o el Cifrado del lado del servidor con la opción Claves proporcionadas por el cliente (SSE-C) para cifrar los datos almacenados en reposo. {{site.data.keyword.cos_full_notm}} proporciona la tecnología de cifrado para SSE y SSE-C. De forma alternativa, puede utilizar sus propias bibliotecas de cifrado para cifrar datos antes de almacenarlos en el Cloud Object Storage.

Puede utilizar los mecanismos de control de acceso siguientes para proteger los datos en {{site.data.keyword.cos_full_notm}}.

**Políticas de Identity and Access Management (IAM)**
{: #iam-cos}

IAM permite a las organizaciones con varios empleados crear y gestionar varios usuarios bajo una sola cuenta. Con las políticas de IAM, las empresas pueden otorgar a los usuarios de IAM el control a sus receptáculos de {{site.data.keyword.cos_short}}.

**Listas de control de acceso (ACL)**
{: #acls-cos}

Con las ACL, puede otorgar permisos específicos (por ejemplo, READ, WRITE), a usuarios específicos para un receptáculo individual.

Los datos en reposo se cifran con el cifrado estándar Advanced Encryption Standard (AES) de 256 bits del lado del proveedor automático y el hash Secure Hash Algorithm (SHA)-256. Los datos en movimiento se protegen utilizando Transport Layer Security (TLS), Secure Sockets Layer (SSL) o SNMPv3 con calidad de proveedor incorporados con el cifrado AES.

### Cifrado
{: #encryption-cos}

Los datos en reposo se cifran con el cifrado estándar Advanced Encryption Standard (AES) de 256 bits del lado del proveedor automático y el hash Secure Hash Algorithm (SHA)-256. Los datos en movimiento se protegen utilizando Transport Layer Security/Secure Sockets Layer (TLS/ SSL) o SNMPv3 con calidad de proveedor incorporados con el cifrado AES.

El cifrado del lado del servidor está siempre en ON para los datos de cliente. En comparación con el hashing que se requiere en la autenticación, y la codificación de borrado, el cifrado no es una parte significativa del coste de procesamiento de Cloud Object Storage.

Puede proporcionar su propia clave para el cifrado, ya que SSE-C está soportado en {{site.data.keyword.cos_full_notm}}. Sin embargo, la responsabilidad es suya para gestionar y proporcionar la clave para almacenar y recuperar los datos.

## Resiliencia
{: #resiliency-cos}

Al crear un receptáculo, debe seleccionar un nivel de resiliencia (varias regiones o regional). En función de su selección, los datos se dispersan y se almacenan en más de una ubicación geográfica.

La resiliencia regional es para latencia baja y los datos se distribuyen en tres centros dentro de una sola región. Utilice la resiliencia de varias regiones cuando la disponibilidad resulte crítica, ya que los datos se almacenan en 3 o más regiones distintas. El uso de varias regiones ofrece resiliencia geográfica y está disponible en más de un punto final. Tenga en cuenta que la aplicación necesita decidir entre las dos.

### Geografías
{: #geographies-cos}

Puede utilizar {{site.data.keyword.cos_full_notm}} desde cualquier lugar del mundo. Puede elegir dónde desea almacenar los datos en el momento de la creación del receptáculo. La información almacenada en IBM COS está cifrada y se dispersa en más de una ubicación geográfica utilizando tecnologías de almacenamiento distribuidas que proporciona IBM Object Storage System. 

Tenga en cuenta los factores siguientes para seleccionar la ubicación geográfica de su almacén de objetos cuando decida entre opciones regionales y de varias regiones.

**Consideraciones sobre la ubicación geográfica**:
* Una ubicación que es remota de sus operaciones para redundancia.
* Una ubicación para hacer frente a los requisitos legales y normativos.
* Reducir las latencias de acceso a datos.
* Considerar los modelos de precios.

## Casos de uso y clases de almacenamiento
{: #usecases-cos}

En función del caso de uso, puede reducir los costes seleccionando un plan de servicio que se ajuste a sus necesidades. Las operaciones de archivado que implican un acceso mínimo al almacén de objetos no necesitan la velocidad ni la durabilidad de un objeto al que se accede con frecuencia, y esta distinción se refleja en el soporte de Storage Class y en el plan de precios para las aplicaciones. Las clases de almacenamiento se definen en el nivel de receptáculo, de modo que puede utilizar una combinación de planes para que se ajusten a sus necesidades. Cree un grupo establecido en la clase de almacenamiento que desea utilizar.

Encontrará más información sobre el precio en la documentación de [Clase de almacenamiento de {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/help/billing.html#ibm-cos-pricing).

### Clases de almacenamiento de ejemplo
{: #samples-cos}

**Estándar**
Este servicio es para datos no estructurados que requieren acceso frecuente, como por ejemplo, DevOps, colaboración y repositorios de contenido de acciones.

**Caja fuerte**
Este servicio es para las cargas de trabajo con datos a los que se accede con poca frecuencia, como por ejemplo cargas de trabajo de copia de seguridad, archivado y conformidad.

**Caja fuerte fría**
Esta opción de despliegue es ideal para los requisitos de acceso mínimos, la conformidad de registros históricos y la copia de seguridad a largo plazo.

**Flexible** Despliegue para distintos requisitos de acceso a datos y para proteger su presupuesto de fluctuaciones de costes inesperadas.
Las clases de almacenamiento se definen en el nivel de receptáculo. Cree un grupo establecido en la clase de almacenamiento que desea utilizar.
