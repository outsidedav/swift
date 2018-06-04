---

copyright:
  years: 2018
lastupdated: "2018-05-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}

# Step 4: Verifying the database connection

Test your database connection.

### Procedure

To verify your database connection, edit the file `Sources/Application/Application.swift` to add a command to test the database connection. 
For example, add the following command in the `class ApplicationServices`:

<pre><code class="hljs">
class ApplicationServices {
	...
	public init() throws {
		...
		let server = try Server(jsonData.uri)
		mongoDBService = MongoKitten.Database(named: "admin", atServer: server)
		<b>if server.isConnected {
			print("Connected to mongodb: " + String(describing: mongoDBService))</b>
		}
		...
	}
}	
</code></pre>

### What to do next

Once the application has completed deployment in [Step 6](use-step6.html), the following message is displayed, if
your connection to the database succeeds:

<pre><code class="hljs">
...
Connected to mongodb: 
MongoKitten.Database&lt;mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin&gt;
...
</code></pre>
