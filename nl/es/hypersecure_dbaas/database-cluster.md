---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-15"

keywords: swift database, secure database swift, cluster database swift, mongokitten swift, verify database swift, credentials swift, storage api swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:gif: .gif}
{:tip: .tip}

# Creación de una base de datos segura y de alta disponibilidad
{: #create-database-cluster}

Para aprovechar al máximo una base de datos de alta disponibilidad y seguridad, incorpore la lógica adicional en la aplicación. Al utilizar los fragmentos de código proporcionados, puede crear y acceder a una base de datos MongoDB. 

Actualmente, el lenguaje de programación soportado para utilizar {{site.data.keyword.ihsdbaas_full}}
es Swift 4.0 con MongoKitten SDK 4.0.0.

## Paso 1. Creación de un clúster de base de datos
{: #create_dbcluster}

1. Acceda a la pantalla [Configuración del servicio {{site.data.keyword.ihsdbaas_full}}](https://cloud.ibm.com/catalog/services/hyper-protect-dbaas){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

2. Proporcione la siguiente información:

	<dl>
		<dt>Nombre del servicio:</dt>
		<dd>Un nombre para el servicio de base de datos.</dd>

    <dt>Seleccione una región/ubicación de despliegue:</dt>
    <dd>Seleccione el centro de datos en el que se encuentra la base de datos.</dd>

    <dt>Seleccione un grupo de recursos:</dt>
    <dd>Si no se puede seleccionar ningún grupo de recursos, puede crear uno en el panel de control de IBM Cloud.</dd>

		<dt>Nombre de clúster/Replicaset:</dt>
		<dd>Un nombre para el clúster de base de datos.</dd>

		<dt>Nombre del administrador de la base de datos:</dt>
		<dd>Un ID de usuario para el administrador de base de datos (DBA).</dd>

		<dt>Contraseña del administrador de la base de datos:</dt>
		<dd>Una contraseña para el ID de usuario de DBA.</dd>

    <dt>Confirmar contraseña del administrador de la base de datos:</dt>
    <dd>Confirme la contraseña para el ID de usuario de DBA.</dd>

		<dt>Tipo de base de datos:</dt>
		<dd>Seleccione el tipo de base de datos. Actualmente, solo se da soporte a MongoDB.</dd>

    <dt>Acuerdo de licencia:</dt>
    <dd>Lea el acuerdo de licencia y marque el recuadro para confirmar su acuerdo.</dd>

    <dt>Precios:</dt>
		<dd>La solución actual solo proporciona una categoría de precios, que no tiene ningún coste. En las versiones posteriores, puede seleccionar la categoría de precios.</dd>
	</dl>

3. Pulse **Crear**.

  Se muestra el panel de control {{site.data.keyword.cloud_notm}}. Es posible que tenga que renovar el navegador para ver el nuevo clúster, que se muestra en la sección Servicios.</p></li>

4. Cuando seleccione el servicio, se mostrará la información del clúster.

5. En el separador Gestionar de la información del clúster, pulse **ABRIR**.

	Se muestra el panel de control {{site.data.keyword.ihsdbaas_full}}.

6. Obtenga los nombres de host y los números de puerto de las tres instancias de base de datos creadas que pertenecen a su clúster de base de datos. Necesita los nombres de host, los números de puerto y las credenciales de usuario para los pasos de la sección [Conectarse a la base de datos](#connect_db).

## Paso 2. Creación de un proyecto utilizando un kit de inicio
{: #create_starter}

Necesita un kit de inicio que se base en la infraestructura web de Swift del lado del servidor Kitura.

![Detalles de característica](videos/StarterKit.gif){: gif}

Utilice un proyecto existente creado a partir de este kit de inicio, o cree un proyecto nuevo.

1. Abra el [Panel de control de servicio de app de {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/developer/appservice/dashboard){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

2. Seleccione el separador **Kits de inicio**.

3. Seleccione el kit de inicio **Swift Kitura** Backend for Frontend. Asegúrese de no confundirlo con el kit de inicio Swift Kitura Basic web-App Starter Kit.
  Se mostrará la página Crear nuevo proyecto.

4. Especifique los detalles del proyecto y pulse **Crear proyecto**.
  Se mostrará la página del proyecto.

5. En la página de proyectos, pulse **Descargar código**.

6. Expanda el archivo comprimido en el directorio de proyecto.

## Paso 3. Conectarse a la base de datos
{: #connect_db}

Para garantizar la transferencia segura de datos, descargue el archivo de la entidad emisora de certificados (CA) desde
https://api.hypersecuredbaas.ibm.com/cert.pem, and copy it to your project directory.

1. Vaya al directorio del proyecto en el que hay los archivos de código de descarga ampliados.

2. Cree un archivo JSON que se denomine `cred.json` para almacenar las credenciales de acceso en el clúster de base de datos.

3. Especifique los valores que ha obtenido en los pasos del apartado [Creación de un clúster de base de datos](#create_dbcluster). Los valores deben especificarse en una sola línea.
  ```hljs
  {
  "uri": "mongodb://<admin_ID>:<admin_pwd>@<Hostname_1>:<PortNumber_1>,
  <Hostname_2>:<PortNumber_2>,<Hostname_3>:<PortNumber_3>
   /admin?ssl=true&ssl_ca_certs=/swift-project/<CA_file>"
  }
  ```
  {: codeblock}

  Donde:
  <table>
  <tr>
    <th> Parámetro </th>
    <th> Descripción </th>
  </tr>
  <tr>
    <td> &lt;<em>admin_ID</em>&gt; </td>
    <td> Es el ID de usuario del administrador de base de datos tal como se especifica en [Creación de un clúster de base de datos](#create_dbcluster).
  </td>
  </tr>
  <tr>
    <td> &lt;<em>admin_pwd</em>&gt; </td>
    <td> Es el ID de usuario de la contraseña del administrador tal como se especifica en [Creación de un clúster de base de datos](#create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>Hostname_i</em>&gt; </td>
    <td> Es una réplica de base de datos <em>i</em> (<em>i</em>=1,2,3) tal como se devuelve en [Creación de un clúster de base de datos](create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>PortNumber_i</em>&gt; </td>
    <td> Es un número de puerto <em>i</em> (<em>i</em>=1,2,3) tal como se devuelve en [Creación de un clúster de base de datos](#create_dbcluster). </td>
  </tr>
  <tr>
    <td> &lt;<em>CA_file</em>&gt; </td>
    <td> Es el nombre de archivo del archivo CA descargado. Durante el despliegue, se copia en el directorio `/swift-project`.</td>
  </tr>
  </table>

4. Edite el archivo `Package.swift` para añadir dependencias de paquetes para el uso del SDK de MongoKitten.

  * En la sección dependencies, añada la línea siguiente:
   ```swift
   .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"),
   ```
   {: codeblock}

  * En la sección targets, añada la dependencia "MongoKitten" a la línea siguiente. **Nota:** Los valores deben especificarse en una sola línea.
   ```swift
   .target(name: "Application", dependencies: [ "Kitura",
   "CloudEnvironment","SwiftMetrics","Health","MongoKitten", ]),
   ```
   {: codeblock}

5. Edite el archivo `Sources/Application/Application.swift` para inicializar la conectividad con MongoDB utilizando MongoKitten.

  * Importe el SDK de MongoKitten:
    ```swift
	  import MongoKitten
	  ```
	  {: codeblock}

  * Añada la clase `ApplicationServices`:
    ```swift
	  cclass ApplicationServices {
	  /* Service references */
	  public let mongoDBService: MongoKitten.Database
	  public let myCredFile = "/swift-project/cred.json"

    public init() throws {
        /* Leer credenciales del archivo cred.json */
        struct ResponseData: Decodable {
            var uri: String
	        }
	        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
	        let decoder = JSONDecoder()
	        let jsonData = try decoder.decode(ResponseData.self, from: data!)

        /* Ejecutar inicializadores de servicios */
        let server = try Server(jsonData.uri)
        mongoDBService = MongoKitten.Database(named: "admin", atServer: 		server)
    }
	}
	```
	{: codeblock}

  * En la clase pública `App`, añada las líneas siguientes para inicializar la conexión de base de datos:
    ```swift
	  public class App {
	  ...
	  let services: ApplicationServices

	  public init() throws {
	  /* Servicios */
	  services = try ApplicationServices()
	  }
	  ...
    ```
    {: codeblock}

## Paso 4. Verificación de la conexión de base de datos
{: #verify_database}

1. Verifique la conexión de base de datos editando el archivo `Sources/Application/Application.swift` para añadir un mandato para probar la conexión de base de datos.
Por ejemplo, añada el mandato siguiente en la `clase ApplicationServices`:

	```swift
		class ApplicationServices {
		    ...
		    public init() throws {
		        ...
		        let server = try Server(jsonData.uri)
		        mongoDBService = MongoKitten.Database(named: "admin", atServer: server)
		        if server.isConnected {
		            print("Connected to mongodb: " + String(describing: mongoDBService))
		        }
		        ...
		    }
		}
	```
	{: codeblock}

Después de desplegar la aplicación en el [Paso 6](#use-step6), se visualiza el siguiente mensaje, si la
conexión a la base de datos se realiza correctamente:

```
...
Connected to mongodb:
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
```
{: screen}

## Paso 5. Incorporación del código de la aplicación
{: #embed_appcode}

Ahora puede añadir su propio código de aplicación al proyecto. Para obtener más información, consulte la documentación de la
[API de MongoKitten](http://beta.openkitten.org/tutorials/){: new_window} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo").

## Paso 6. Despliegue de la aplicación
{: #deploy-dbcluster}

Puede ejecutar la aplicación [en local](/docs/swift?topic=swift-swift_cli#swift-install-tools) con las herramientas de compilación necesarias, o bien desplegar a {{site.data.keyword.cloud_notm}}.

Para crear una cadena de herramientas de despliegue en el panel de control, pulse **Desplegar**. Configure el destino de despliegue de acuerdo con las instrucciones correspondientes al método que elija:
  * **Desplegar en
[{{site.data.keyword.containerlong}}](/docs/apps/deploying?topic=creating-apps-containers-kube#containers)**. Esta opción crea un clúster de hosts, llamado nodos de trabajador, para desplegar y gestionar contenedores de aplicaciones de alta disponibilidad. Puede crear un clúster o desplegar en un clúster existente.
  * **Desplegar en Cloud Foundry**. Esta opción despliega la app nativa de cloud sin la necesidad de gestionar la infraestructura subyacente. Si su cuenta tiene acceso a {{site.data.keyword.cfee_full_notm}}, puede seleccionar el tipo de desplegador **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf#about-cf)** o **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee#cfee)**, que puede utilizar para crear y gestionar entornos aislados para el alojamiento de aplicaciones Cloud Foundry exclusivamente para su empresa.
  * **Desplegar en un [Servidor virtual](/docs/apps?topic=creating-apps-vsi-deploy#vsi-deploy)**. Esta opción proporciona una instancia de servidor virtual, carga una imagen que incluye su app, crea una cadena de herramientas DevOps e inicia el primer ciclo de despliegue automáticamente.
