---

copyright:
  years: 2018
lastupdated: "2018-03-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}


# Step 3: Connecting to the database

Connect a Swift application to the MongoDB instance using the MongoKitten SDK.

## Before you begin

To ensure secure data transfer, download the Certificate Authority (CA) file from
https://api.hypersecuredbaas.ibm.com/cert.pem and copy it to your project directory.

## Procedure

<ol>
<li> Change to your project directory, which contains the expanded download code
files. </li>

<li> Create a JSON file named `cred.json` to store your
access credentials to the database cluster.<br>
<p> Enter the values you obtained as a result of [Step 1](use-step1.html).<br>
**Note:** Please be aware that the values must be specified in a single line. 

<table>
  <tr>
    <td>
	<pre><code class="hljs">{
"uri": "mongodb:/&sol;&lt;<em>admin_ID</em>&gt;&colon;&lt;<em>admin_pwd</em>&gt;@&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,
&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;
/admin?ssl=true&ssl_ca_certs=/swift-project/&lt;<em>CA_file</em>&gt;"
}</code></pre>
	</td>
  </tr>
</table>
Where:

<table>
  <tr>
    <th> Parameter </th>
    <th> Description </th>
  </tr>
  <tr>
    <td> &lt;<em>admin_ID</em>&gt; </td>
    <td> Is the user ID of the database administrator as specified in [Step 1](use-step1.html).
  </td>
  </tr>
  <tr>
    <td> &lt;<em>admin_pwd</em>&gt; </td>
    <td> Is the user ID of the administrator password as specified in [Step 1](use-step1.html). </td>
  </tr>
  <tr>
    <td> &lt;<em>Hostname_i</em>&gt; </td>
    <td> Is a database replica <em>i</em> (<em>i</em>=1,2,3) as returned in [Step 1](use-step1.html). </td>
  </tr>
  <tr>
    <td> &lt;<em>PortNumber_i</em>&gt; </td>
    <td> Is a port number <em>i</em> (<em>i</em>=1,2,3) as returned in [Step 1](use-step1.html). </td>
  </tr>
  <tr>
    <td> &lt;<em>CA_file</em>&gt; </td>
    <td> Is the file name of the downloaded CA file. During deployment, it will be copied to the directory `/swift-project`.</td>
  </tr>
</table>
</p>
</p>
</li>

<li> Edit the `Package.swift` file to add package dependencies for the use of the
MongoKitten SDK.
	<ol>
		<li> In the dependencies section, add the following line:
			<table>
				<tr>
					<td>
						<pre><code class="hljs"> .package(url: "https://github.com/OpenKitten/MongoKitten.git", from: "4.0.0"), </code></pre>
					</td>
				</tr>
			</table>
		</li>
		
		<li> In the targets section, add the dependency "MongoKitten" to the following line.<br>
		<br>**Note:** Please be aware that the values must be specified in a single line.
			<table>
				<tr>
					<td>
						<pre><code class="hljs"> .target(name: "Application", dependencies: [ "Kitura", 
						"CloudEnvironment","SwiftMetrics","Health",<b>"MongoKitten",</b> ]),</code></pre>
					</td>
				</tr>
			</table>
		</li>
	</ol>
</li>

<li> Edit the `Sources/Application/Application.swift` file to initialize a connectivity
to MongoDB using MongoKitten.
	<ol>
		<li> Import the MongoKitten SDK:
			<table>
				<tr>
					<td>
						<pre> import MongoKitten </pre>
					</td>
				</tr>
			</table>
		</li>
		
		<li> Add the class `ApplicationServices`:
			<table>
				<tr>
					<td>
						<pre><code class="hljs"> class ApplicationServices {
    // Service references
    public let mongoDBService: MongoKitten.Database
    public let myCredFile = "/swift-project/cred.json"

    public init() throws {
        // Read credentials from json file cred.json
        struct ResponseData: Decodable {
            var uri: String
        }
        let data = try? Data(contentsOf: URL(fileURLWithPath: myCredFile))
        let decoder = JSONDecoder()
        let jsonData = try decoder.decode(ResponseData.self, from: data!)
 
        // Run service initializers
        let server = try Server(jsonData.uri)
        mongoDBService = MongoKitten.Database(named: "admin", atServer: server)
    }
}</code></pre>
					</td>
				</tr>
			</table>
		</li>
		
		<li> In the public class `App`, add the following lines to initialize the database connection:
			<table>
				<tr>
					<td>
						<pre><code class="hljs"> public class App {
    ...
    <b>let services: ApplicationServices</b>

    public init() throws {
        <b>// Services
        services = try ApplicationServices()</b>
        
    }
    ...</code></pre>
					</td>
				</tr>
			</table>
		</li>
	</ol>
</li>
</ol>

## What to do next

Proceed with [Step 4](use-step4.html).
