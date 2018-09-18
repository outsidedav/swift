---

copyright:
  years: 2018
lastupdated: "2018-09-18"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Logging in Swift
{: #logging_swift}

Log messages are strings with contextual information about the state and activity of the microservice at the time that the log entry is made. Logs are required to diagnose how and why services fail, and plays a supporting role to [appmetrics](appmetrics.html) in monitoring application health.

Given the transient nature of processes in Cloud environments, logs must be collected and sent elsewhere, usually to a centralized location for analysis. The most consistent way to log in cloud environments is to send log entries to standard output and error streams, which leaves the infrastructure to handle the rest.


## Adding Logging to your Swift app

[HeliumLogger](https://github.com/IBM-Swift/HeliumLogger) is a popular lightweight logging framework for Swift, and provides many native benefits such as logging to standard output and different log levels.

[LoggerAPI](https://github.com/IBM-Swift/LoggerAPI) is the logger protocol that provides a common logging interface for different kinds of loggers in Swift. Kitura uses the `LoggerAPI` throughout its implementation.

To use `HeliumLogger`, add the following code to the **dependencies:** in your `Package.swift` for all appropriate targets:
```swift
.package(url: "https://github.com/IBM-Swift/HeliumLogger.git", from: "1.7.1")
```
{: codeblock}

Add the following instrumentation code to initialize `HeliumLogger` and set it as the logger used by the `LoggerAPI`:
```swift
import HeliumLogger
import LoggerAPI

let logger = HeliumLogger(.verbose)
Log.logger = logger

Log.info("This is an informational log message.")
```
{: codeblock}

In the provided example, the [log level](http://ibm-swift.github.io/HeliumLogger/) was explicitly set to `.verbose`, which is the default.

For more information about customizing the log messages, see the official [HeliumLogger API reference documentation](http://ibm-swift.github.io/HeliumLogger/).

## Logging with StarterKits
{: #monitoring}

Swift apps that are created by using the {{site.data.keyword.cloud_notm}} App Service come with `HeliumLogger` by default. Running the app natively or in a cloud environment produces the following output:
```
[2018-07-31T15:41:05.332-05:00] [INFO] [HTTPServer.swift:195 listen(on:)] Listening on port 8080.
```
{: screen}

These messages are found in `stdout` locally, or in the logs for [CloudFoundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs) and [Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/) deployments, which are accessed by `ibmcloud app logs --recent <APP_NAME>` and `kubectl logs <deployment name>`.

In the `/Sources/AppName/main.swift` file, you can see the following code:
```swift
HeliumLogger.use(LoggerMessageType.info)
```
{: codeblock}

The log level is explicitly set to `.info` to log informational level messages like the application startup logs.
{: tip}

## Next Steps
{: #next_steps}

Learn more about viewing the logs in each of our deployment environments:
* [Kubernetes Logs](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_logs/)
* [Cloud Foundry Logs](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [{{site.data.keyword.openwhisk}} Logs & Monitoring](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Using a Log aggregator:
* [{{site.data.keyword.cloud_notm}} Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK stack](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
