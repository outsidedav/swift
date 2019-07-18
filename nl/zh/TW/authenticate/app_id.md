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

# 新增使用者鑑別
{: #appid}

應用程式安全非常複雜。對於大部分開發人員而言，這是建立應用程式較困難的作業之一。如何才能確定您正在保護使用者資訊？藉由將 {{site.data.keyword.appid_full}} 整合至您的應用程式，即可保護資源並新增鑑別，即使您沒有太多的安全經驗也可以做到。

若要求使用者登入，即可儲存使用者資料，例如，應用程式喜好設定（或公用社交設定檔中的資訊），然後使用該資料來自訂應用程式內的每個使用者經驗。{{site.data.keyword.appid_short_notm}} 為您提供一個登入架構，但是您也可以帶入自創品牌的登入畫面，以與雲端目錄搭配使用。

如需可以使用 {{site.data.keyword.appid_short_notm}} 的所有方式以及架構資訊，請參閱[關於 {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-about#about)。

## 開始之前
{: #prereqs-appid}

首先，請確定您具備下列必要條件：
* CocoaPods（1.1.0 版或更新版本）
* iOS（第 9 版或更新版本）
* MacOS（10.11.5 版或更新版本）
* Xcode（9.0.1 版或更新版本）

## 步驟 1. 建立 {{site.data.keyword.appid_short_notm}} 實例
{: #create-instance-appid}

建立 {{site.data.keyword.appid_short_notm}} 服務的實例：

1. 在 [{{site.data.keyword.cloud_notm}} 型錄](https://{DomainName}/catalog){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 中，選取 {{site.data.keyword.appid_short_notm}}。即會開啟服務配置畫面。
2. 提供服務實例的名稱，或使用預設名稱。
3. 選取定價方案，然後按一下**建立**。

## 步驟 2. 安裝 iOS Swift SDK
{: #install-sdk-appid}

此服務提供 SDK，以協助您輕鬆地以程式碼撰寫應用程式。SDK 必須安裝在應用程式程式碼中。

1. 開啟現有 Xcode 專案目錄的 `Podfile`。

2. 在您的專案目標下，新增 `IBMCloudAppID` Pod 的相依關係。請確定 `use_frameworks!` 指令也在您的目標下，如下列範例所示。
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'IBMCloudAppID'
    end
    ```
    {: pre}

3. 下載 `IBMCloudAppID` 相依關係。
    ```
    pod install --repo-update
    ```
    {: pre}

## 步驟 3. 起始設定 SDK
{: #initialize-sdk-appid}

在應用程式中起始設定 SDK 之後，您可以開始配置 {{site.data.keyword.appid_short_notm}} 喜好設定。

1. 移至**專案設定 > 功能 > 金鑰鏈共用**，然後啟用 Xcode 專案的金鑰鏈共用。

2. 移至**專案設定 > 資訊 > URL 類型**，將下列值新增至 **URL 架構**及 **ID** 文字框中。
    
  ```
    $(PRODUCT_BUNDLE_IDENTIFIER)
    ```
  {: codeblock}

3. 將下列 import 新增至 `AppDelegate.swift` 檔案。
    
  ```swift
  import IBMCloudAppID
  ```
  {: codeblock}

4. 傳遞 `tenantID` 和 `AppID_region` 參數，以起始設定 SDK。放置程式碼的一般工作區（但非一定）位於您應用程式中 `AppDelegate` 的 `application:didFinishLaunchingWithOptions` 方法中。
  ```swift
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, <AppID_region>);
  ```
  {: codeblock}
  
  * `tenantID`：租戶 ID 是用於起始設定應用程式的唯一 ID。在 {{site.data.keyword.appid_short_notm}} 儀表板中，透過選取**服務認證**標籤，然後按一下**檢視認證**，可以找到該值。
  * `AppID_region`：{{site.data.keyword.appid_short_notm}} 地區是您要在其中使用服務的 {{site.data.keyword.cloud_notm}} 地區。此地區可以在服務儀表板中找到，可以是 `AppID.REGION_US_SOUTH`、`AppID.REGION_SYDNEY` 和 `AppID.REGION_UK`。

5. 將下列代碼新增到 `AppDelegate` 檔案。
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: codeblock}

## 步驟 4. 管理登入經驗
{: #managing-signin-appid}

身分提供者會為您的使用者提供鑑別資訊，如此您就可以授權給他們。使用 {{site.data.keyword.appid_short_notm}}，您可以使用社交身分提供者（例如，Facebook 及 Google+），或者您可以使用雲端目錄來管理使用者登錄。

您隨時都可以更新配置，而不需要更新應用程式程式碼。
{: tip}

### 社交身分提供者
{: #social-appid}

使用 {{site.data.keyword.appid_short_notm}}，您可以使用社交身分提供者（例如，Facebook 及 Google+）來保護您的應用程式。

若要配置社交身分提供者，請執行下列動作：

1. 開啟 {{site.data.keyword.appid_short_notm}} 儀表板的**身分提供者 > 管理**。
2. 將您要使用的身分提供者設為**開啟**。您可以使用任何組合的身分提供者，但是如果您要帶入自訂的登入畫面，則需要僅啟用「雲端目錄」。
3. 將[預設配置](/docs/services/appid?topic=appid-social#social)更新為您自己的認證。{{site.data.keyword.appid_short_notm}} 提供 IBM 認證，您可以用來試用服務，但在發佈您的應用程式之前，需要更新配置。
4. 自訂登入畫面，以顯示您選擇的影像及顏色。
5. 若要使用您的應用程式來呼叫登入小組件，請將下列指令新增至您的程式碼。
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

### 雲端目錄
{: #cloud-dir-appid}

使用 {{site.data.keyword.appid_short_notm}}，您可以管理自己的使用者登錄，稱為雲端目錄。雲端目錄可讓使用者使用其電子郵件及密碼，來註冊並登入行動及 Web 應用程式。

若要帶入您自創品牌的使用者介面畫面，只有雲端目錄可以啟用作為身分提供者。
{: tip}

若要配置雲端目錄，請執行下列動作：

1. 開啟 {{site.data.keyword.appid_short_notm}} 儀表板的**管理身分提供者**標籤，然後將雲端目錄設為**開啟**。
2. 配置[目錄及訊息設定](/docs/services/appid?topic=appid-cloud-directory)。
4. 選擇您要顯示的登入畫面組合，並在應用程式中放置程式碼以呼叫那些畫面。
    * 登入
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

    * 註冊
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

    * 忘記密碼
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

    * 帳戶詳細資料
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

    * 變更密碼
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


## 步驟 5. 測試應用程式
{: #testing-appid}

所有項目都正確配置嗎？您可以測試看看！

1. 以 Xcode 模擬器開啟應用程式。
2. 使用 GUI，逐步進行登入應用程式的處理程序。如果您已配置雲端目錄，請確定所有畫面的顯示都如您所願。
3. 更新 {{site.data.keyword.appid_short_notm}} 儀表板中的身分提供者或登入小組件畫面。
4. 重複步驟 1 和 2，查看變更是否立即實作。不需要更新應用程式程式碼。

有困難嗎？請參閱[疑難排解 {{site.data.keyword.appid_short_notm}}](/docs/services/appid?topic=appid-troubleshooting)。

## 後續步驟
{: #next-appid notoc}

做得好！您已為應用程式新增一個安全等級。嘗試下列其中一個選項，以保持動力：

* 進一步瞭解並充分運用 {{site.data.keyword.appid_short_notm}} 提供的所有特性，[請參閱文件](/docs/services/appid?topic=appid-getting-started#getting-started)！
* 「入門範本套件」是使用 {{site.data.keyword.cloud_notm}} 特性最快的方式之一。請檢視[行動開發人員儀表板](https://{DomainName}/developer/mobile/dashboard){: new_window} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 中的可用入門範本套件。下載程式碼。執行應用程式！
