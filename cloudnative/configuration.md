---

copyright:
  years: 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuring the Swift environment
{: #healthcheck}

Ideally, a Cloud application is able to move from one environment to another (for example, from testing into production) without changing code, or exercising otherwise untested code paths.

Problems can arise when significant differences exist in the way the configuration is presented, depending on the development environment. For example, CloudFoundry, which uses stringified JSON objects versus Kubernetes that uses either flat values or stringified JSON objects. Local development has different considerations as well. Credentials can be presented differently across public and private, which further make it difficult for apps to remain unchanged across environments.

Standardized guidelines are available to follow for developing Swift applications that help facilitate and maintain consistent portability. Considerations to make include credential management, saving data, storing data, and publishing content to the cloud.

### General requirements
* Must be able to inject configuration at run time (minimally through an environment variable).
* Application code can use or look up a value by using a consistent string key.
* Application code can use credentials that are provided by the platform for the target environment.

Whether you need to add Cloud support to existing applications or create apps with Starter kits, the goal is to provide maximum portability for Swift apps regardless of the development platform.

### {{site.data.keyword.cloud_notm}} requirements
* Consistent {{site.data.keyword.cloud_notm}} mechanism for providing credentials to applications, like service instances that use common (Open) service broker.
* Consistent pattern for saving, storing, and publishing environment-specific configuration.
  * Using `kubeconfig` and context elements.
  * References to secrets (service instances) in helm, as seen in this [example](https://github.com/kubernetes/helm/issues/2780).

## Adding IBM Cloud support to existing Swift applications
{: #addcloud-env}

The [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env) library abstracts environment configuration and credentials from various Cloud compute providers. This way your Swift app can consistently access the information running locally or in Cloud Foundry, Kubernetes, and {{site.data.keyword.openwhisk}}. The credentials abstraction is directly in `CloudEnvironment` itself, while `CloudEnvironment` uses [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv) for the Cloud Foundry configuration and [Configuration](https://github.com/IBM-Swift/Configuration) as a configuration manager.

The path for obtaining certain environment values can differ from one cloud environment to another. By leveraging this `CloudEnvironment`, you can make your Swift application environment-agnostic when it comes to obtaining such values. With `CloudEnvironment`, you can abstract low-level details from your application's source code by defining a lookup key that your Swift application can leverage for searching its corresponding value.

### Abstraction and service credentials
By using the `CloudEnvironment` library, you can define an array of search patterns for looking up a JSON object that is mapped to environment variables, such as service credentials. Each element in the search patterns array are *executed* until the variable is found. `CloudEnvironment` supports searching for values by using the following three search pattern types:

- `cloudfoundry` - A pattern type used to search for a value in Cloud Foundry's services environment variable (for example, `VCAP_SERVICES`).
- `env` - A pattern type used to search for a value that is mapped to an environment variable, as in Kubernetes or Functions.
- `file` - A pattern type used to search for a value in a JSON file.

You can specify lookup keys and search patterns in a file named `mappings.json`. This file must exist in a `config` folder under the root folder of your Swift project. The following shows an example of a `mappings.json` file:

```javascript
{
    "cloudant-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-cloudant-db",
                "env:my_awesome_cloudant_db_credentials",
                "file:localdev/my-awesome-cloudant-db-credentials.json"
            ]
        }
    },
    "object-storage-credentials": {
        "credentials": {
            "searchPatterns": [
                "cloudfoundry:my-awesome-object-storage",
                "env:my_awesome_object_storage_credentials",
                "file:localdev/my-awesome-object-storage-credentials.json"
            ]
        }
    }
}
```
{: codeblock}

In this example, `cloudant-credentials` and `object-storage-credentials` are the lookup keys that your Swift application uses to look up the corresponding credentials. Note that the path next to the `file` search pattern must be relative to the root folder of your Swift application. The `file:` search pattern is useful for providing credentials to your locally running application. For security purposes, credential files can not be added to repositories.

To leverage the `CloudEnvironment` package in your Swift application, specify a dependency for it in your `Package.swift` file:
```swift
   dependencies: [
   ...
       .package(url: "https://github.com/IBM-Swift/CloudEnvironment.git", .upToNextMajor(from: "8.0.0")),
       ...
   ]``
{: codeblock}

Then add the following instrumentation code to your application:
```swift
import CloudEnvironment

...

let cloudEnv = CloudEnv()

...

let cloudantCredentials = cloudEnv.getCloudantCredentials(name: "cloudant-credentials")
// cloudantCredentials.username, cloudantCredentials.password, cloudantCredentials.url, etc.
let objStorageCredentials = cloudEnv.getObjectStorageCredentials(name: "object-storage-credentials")
// objStorageCredentials.username, objStorageCredentials.password, objStorageCredentials.projectID, etc.
...

let service1Credentials = cloudEnv.getDictionary("service1-credentials")
let service1CredentialsStr = cloudEnv.getString("service1-credentials")

...

// Get a default PORT and URL
let port = cloudEnv.port
let url = cloudEnv.url

...
```
{: codeblock}

This example provides access to the credential sets for services, which can now be used to initialize connections to these services. Check out [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv#api) for specific CloudFoundry configuration and [Configuration](https://github.com/IBM-Swift/Configuration) for details on loading configuration data.

## Using the Swift configuration manager from Starter Kit apps

Swift apps that are created with [Starter Kits](https://console.bluemix.net/developer/appledevelopment/starter-kits/) automatically come with the credentials and configuration needed to run locally, and also in many Cloud deployment environments (CF, K8s, VSI, and Functions). The basic creation of the configuration manager can be found in `Sources/Application/Application.swift`.

### Understanding service credentials

If you are binding credentials to services, your application configuration information for services is stored in the `localdev-config.json` file in the `/config` directory. The file is in the `.gitignore` directory to prevent sensitive information from being stored in git. The connection information for any configured services running locally, such as username, password, and hostname, is stored in this file.

The application uses the configuration manager to read the connection and configuration information from the environment, and this file. It uses a custom built `mappings.json`, found in the `config` directory, to communicate where the credentials can be found for each service. If the application is running locally, it can connect to {{site.data.keyword.cloud_notm}} services by using unbound credentials read from this file. When you push your application to {{site.data.keyword.cloud_notm}}, these values are no longer used. Instead, the application automatically connects to bound services by using environment variables.


## Next Steps

Check out our three libraries to help your applications abstract themselves from their environments:

* [CloudEnvironment](https://github.com/ibm-developer/ibm-cloud-env)
* [Swift-cfenv](https://github.com/IBM-Swift/Swift-cfenv)
* [Configuration](https://github.com/IBM-Swift/Configuration)
