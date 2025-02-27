---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-19"

keywords: authentication swift, security swift, forgot password swift, social swift, identity provider swift, tentantid swift, cloud directory swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 添加用户认证
{: #appid}

应用程序安全性非常复杂。对于大多数开发者来说，应用程序安全性是创建应用程序过程中最难解决的问题之一。如何确保用户信息能够得到安全保护？通过将 {{site.data.keyword.appid_full}} 集成到应用程序中，您可以保护资源并添加认证，即使您没有太多安全方面的经验，也没关系。

通过要求用户登录，您可以存储用户数据，例如应用程序首选项（或公共社交个人档案中的信息），然后使用这些数据为每个用户定制应用程序体验。{{site.data.keyword.appid_short_notm}} 提供了一个登录框架，但您也可以使用自己的特色登录屏幕与云目录配合使用。

有关可以使用 {{site.data.keyword.appid_short_notm}} 的所有方法以及体系结构信息，请参阅[关于 {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-about#about)。

## 开始之前
{: #prereqs-appid}

首先，请确保您已准备好以下必备软件：
* CocoaPods（V1.1.0 或更高版本）
* iOS（V9 或更高版本）
* MacOS（V10.11.5 或更高版本）
* Xcode（V9.0.1 或更高版本）

## 步骤 1. 创建 {{site.data.keyword.appid_short_notm}} 的实例
{: #create-instance-appid}

创建 {{site.data.keyword.appid_short_notm}} 服务的实例：

1. 在 [{{site.data.keyword.cloud_notm}} 目录](https://{DomainName}/catalog){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 中，选择 {{site.data.keyword.appid_short_notm}}。这将打开服务配置屏幕。
2. 为服务实例提供名称或使用预设名称。
3. 选择价格套餐，然后单击**创建**。

## 步骤 2. 安装 iOS Swift SDK
{: #install-sdk-appid}

服务提供了 SDK，用于帮助您更轻松地对应用程序进行编码。该 SDK 必须安装在应用程序代码中。

1. 打开现有 Xcode 项目目录下的 `Podfile`。

2. 在项目目标下，为 `IBMCloudAppID` pod 添加依赖项。确保 `use_frameworks!` 命令也位于目标下，如以下示例中所示。
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'IBMCloudAppID'
    end
    ```
    {: pre}

3. 下载 `IBMCloudAppID` 依赖项。
    ```
  pod install --repo-update
  ```
    {: pre}

## 步骤 3. 初始化 SDK
{: #initialize-sdk-appid}

在应用程序中初始化 SDK 后，可以开始配置 {{site.data.keyword.appid_short_notm}} 首选项。

1. 转至**项目设置 > 功能 > 密钥链共享**，并在 Xcode 项目中启用密钥链共享。

2. 转至**项目设置 > 信息 > URL 类型**，并将以下值添加到 **URL 方案**和**标识**文本框。
  ```
  $(PRODUCT_BUNDLE_IDENTIFIER)
  ```
  {: codeblock}

3. 将以下 import 语句添加到 `AppDelegate.swift` 文件中。
  ```swift
  import IBMCloudAppID
  ```
  {: codeblock}

4. 传递 `tenantID` 和 `AppID_region` 参数，以初始化 SDK。代码通常会放置在应用程序中 `AppDelegate` 的 `application:didFinishLaunchingWithOptions` 方法中，但这并不是强制性的。
  ```swift
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, <AppID_region>);
  ```
  {: codeblock}
  
  * `tenantID`：租户标识是用于初始化应用程序的唯一标识。在 {{site.data.keyword.appid_short_notm}} 仪表板中，通过选择**服务凭证**选项卡，然后单击**查看凭证**，可以找到该值。
  * `AppID_region`：{{site.data.keyword.appid_short_notm}} 区域是您要在其中使用服务的 {{site.data.keyword.cloud_notm}} 区域。此区域可以在服务仪表板中找到，可以是 `AppID.REGION_US_SOUTH`、`AppID.REGION_SYDNEY` 和 `AppID.REGION_UK`。

5. 将以下代码添加到 `AppDelegate` 文件。
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: codeblock}

## 步骤 4. 管理登录体验
{: #managing-signin-appid}

身份提供者为您的用户提供认证信息，以便您可以对这些用户进行授权。通过 {{site.data.keyword.appid_short_notm}}，可以使用社交身份提供者（例如，Facebook 和 Google+），也可以使用 Cloud Directory 来管理用户注册表。

您可以随时更新配置，而不必更新应用程序代码。
{: tip}

### 社交身份提供者
{: #social-appid}

通过 {{site.data.keyword.appid_short_notm}}，可以使用社交身份提供者（例如，Facebook 和 Google+）来保护应用程序。

要配置社交身份提供者，请执行以下操作：

1. 打开 {{site.data.keyword.appid_short_notm}} 仪表板至**身份提供者 > 管理**。
2. 将要使用的身份提供者设置为**开启**。可以使用身份提供者的任意组合，但如果要显示定制的登录屏幕，那么只需启用 Cloud Directory。
3. 将[缺省配置](/docs/services/appid?topic=appid-social#social)更新为您自己的凭证。{{site.data.keyword.appid_short_notm}} 提供了 IBM 凭证，您可以使用这些凭证来试用服务，但在发布应用程序之前，需要更新此配置。
4. 定制登录屏幕以显示您选择的图像和颜色。
5. 要使用应用程序调用登录窗口小部件，请将以下命令添加到代码中。
    ```swift
    import IBMCloudAppID
    class delegate : AuthorizationDelegate {
        public func onAuthorizationSuccess(accessToken: AccessToken, identityToken: IdentityToken, response:Response?) {
            //User authenticated
        }

        public func onAuthorizationCanceled() {
         //Authentication cancelled by the user
        }

        public func onAuthorizationFailure(error: AuthorizationError) {
            //Exception occurred
        }
    }

    AppID.sharedInstance.loginWidget?.launch(delegate: delegate())
    ```
    {: codeblock}

### 云目录
{: #cloud-dir-appid}

通过 {{site.data.keyword.appid_short_notm}}，您可以管理自己的用户注册表（称为云目录）。通过云目录，用户可以使用自己的电子邮件和密码注册并登录到移动和 Web 应用程序。

要显示自己的特色 UI 屏幕，只能将云目录作为身份提供者启用。
{: tip}

要配置云目录，请执行以下操作：

1. 将 {{site.data.keyword.appid_short_notm}} 仪表板打开到**管理身份提供者**选项卡，并将云目录设置为**开启**。
2. 配置[目录和消息设置](/docs/services/appid?topic=appid-cloud-directory)。
4. 选择要显示的登录屏幕的组合，并在应用程序中放入相应代码以调用这些屏幕。
    * 登录
        ```swift
        class delegate : TokenResponseDelegate {
            public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
            /* User authenticated */
            }

            public func onAuthorizationFailure(error: AuthorizationError) {
            /* Exception occurred */
            }
        }

        AppID.sharedInstance.obtainTokensWithROP(username: username, password: password, delegate: delegate())
        ```
        {: codeblock}

    * 注册
        ```swift
        class delegate : AuthorizationDelegate {
          public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             if accessToken == nil && identityToken == nil {
              /* email verification is required */
              return
             }
           /* User authenticated */
          }

          public func onAuthorizationCanceled() {
         /* Sign up cancelled by the user */
          }

          public func onAuthorizationFailure(error: AuthorizationError) {
              /* Exception occurred */
          }
        }

        AppID.sharedInstance.loginWidget?.launchSignUp(delegate: delegate())
        ```
        {: codeblock}

    * 忘记密码
        ```swift
        class delegate : AuthorizationDelegate {
           public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
              /* forgot password finished, in this case accessToken and identityToken will be null. */
           }

           public func onAuthorizationCanceled() {
         /* forgot password canceled by the user */
           }

           public func onAuthorizationFailure(error: AuthorizationError) {
               /* Exception occurred */
           }
        }

        AppID.sharedInstance.loginWidget?.launchForgotPassword(delegate: delegate())
        ```
        {: codeblock}

    * 帐户详细信息
        ```swift

         class delegate : AuthorizationDelegate {
     public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             }

             public func onAuthorizationCanceled() {
         }

             public func onAuthorizationFailure(error: AuthorizationError) {
             }
         }

         AppID.sharedInstance.loginWidget?.launchChangeDetails(delegate: delegate())
        ```
        {: codeblock}

    * 更改密码
        ```swift
         class delegate : AuthorizationDelegate {
             public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             }

             public func onAuthorizationCanceled() {
         }

             public func onAuthorizationFailure(error: AuthorizationError) {
             }
          }

          AppID.sharedInstance.loginWidget?.launchChangePassword(delegate: delegate())
        ```
        {: codeblock}


## 步骤 5. 测试应用程序
{: #testing-appid}

是否一切配置正确？您可以对其进行测试！

1. 使用 Xcode 仿真器打开应用程序。
2. 使用 GUI 逐步完成登录到应用程序的过程。如果已配置云目录，请确保所有屏幕都按您预期的方式显示。
3. 在 {{site.data.keyword.appid_short_notm}} 仪表板中更新身份提供者或登录窗口小部件屏幕。
4. 重复步骤 1 和 2，以查看更改是否立即实施。无需更新应用程序代码。

遇到困难？请查看[{{site.data.keyword.appid_short_notm}} 故障诊断](/docs/services/appid?topic=appid-troubleshooting)。

## 后续步骤
{: #next-appid notoc}

太棒了！您已将安全级别添加到应用程序。请一鼓作气尝试下列其中一个选项：

* 要了解有关 {{site.data.keyword.appid_short_notm}} 必须提供的所有功能的更多信息以及如何利用这些功能，请[查看文档](/docs/services/appid?topic=appid-getting-started#getting-started)！
* 入门模板工具包是使用 {{site.data.keyword.cloud_notm}} 功能的最快方法之一。请在[移动开发者仪表板](https://{DomainName}/developer/mobile/dashboard){: new_window} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标") 中查看可用的入门模板工具包。下载代码。运行应用程序！
