---

copyright:
  years: 2018
lastupdated: "2018-08-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 新增使用者鑑別

應用程式安全可能非常複雜。對於大部分開發人員而言，這是建立應用程式最困難的其中一部分。如何才能確定您正在保護使用者資訊？透過將 {{site.data.keyword.appid_full}} 整合至您的應用程式，便可以保護資源並新增鑑別；即使您沒有太多的安全經驗也可以做到。

要求使用者登入，即可儲存使用者資料，例如，應用程式喜好設定（或公用社交設定檔中的資訊），然後利用該資料來自訂應用程式內每一個使用者經驗。{{site.data.keyword.appid_short_notm}} 提供一個登入架構給您，但是您也可以帶入自創品牌的登入畫面，以與雲端目錄搭配使用。

如需可以使用 {{site.data.keyword.appid_short_notm}} 的所有方式以及架構資訊，請參閱[關於 {{site.data.keyword.appid_short_notm}}](/docs/services/appid/about.html)。

## 開始之前

首先，請確定您具備下列必要條件：
 * CocoaPods（1.1.0 版或更高版本）
 * iOS（第 9 版或更高版本）
 * MacOS（10.11.5 版或更高版本）
 * Xcode（9.0.1 版或更高版本）

## 步驟 1. 建立 {{site.data.keyword.appid_short_notm}} 實例

佈建服務實例：

1. 在 [{{site.data.keyword.cloud_notm}} 型錄](https://console.bluemix.net/catalog/)中，選取 {{site.data.keyword.appid_short_notm}}。即會開啟服務配置畫面。
2. 提供服務實例的名稱，或使用預設名稱。
3. 選取定價方案，然後按一下**建立**。

## 步驟 2. 安裝 iOS Swift SDK

此服務提供 SDK 以協助您輕易以程式碼編寫您的應用程式。SDK 必須安裝在您的應用程式程式碼中。

1. 開啟現有 Xcode 專案目錄的 `Podfile`。

2. 在您的專案目標下，新增 `BluemixAppID` pod 的相依關係。請確定 `use_frameworks!` 指令也在您的目標下，如下列範例所示。
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'BluemixAppID'
    end
    ```
    {: pre}

3. 下載 `BluemixAppID` 相依關係。
    ```
    pod install --repo-update
    ```
    {: pre}

## 步驟 3. 起始設定 SDK

在您的應用程式中起始設定 SDK 之後，您可以開始配置您的{{site.data.keyword.appid_short_notm}} 喜好設定。

1. 移至**專案設定 > 功能 > 金鑰鏈共用**，然後啟用 Xcode 專案的金鑰鏈共用。

2. 移至**專案設定 > 資訊 > URL 類型**，將下列值新增至 **URL 架構**及 **ID** 文字框中。
    ```
    $(PRODUCT_BUNDLE_IDENTIFIER)
    ```
    {: pre}

3. 將下列 import 新增至 `AppDelegate.swift` 檔案。
    ```swift
    import BluemixAppID
    ```
    {:pre}

4. 傳遞承租戶 ID 及地區參數以起始設定 SDK。放置程式碼的一般工作區（但非一定）位於您應用程式中 AppDelegate 的 `application:didFinishLaunchingWithOptions` 方法中。
    ```swift
    AppID.sharedInstance.initialize(tenantId: <tenantId>, bluemixRegion: <AppID_region>)
    ```
    {: pre}
  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt=""/>瞭解指令元件</th>
    </thead>
    <tbody>
      <tr>
        <td><em>tenantID</em></td>
        <td>承租戶 ID 是一個唯一 ID，用來起始設定應用程式。您可以在 {{site.data.keyword.appid_short_notm}} 儀表板中找到此值。在<b>服務認證</b>標籤中，按一下<b>檢視認證</b>。</td>
      </tr>
      <tr>
        <td><em>AppID_region</em></td>
        <td>{{site.data.keyword.appid_short_notm}} 地區是您在其中使用服務的 IBM Cloud 地區。此地區可在服務儀表板中找到，且可為 <em>AppID.REGION_US_SOUTH</em>、<em>AppID.REGION_SYDNEY</em>、<em>AppID.REGION_UK</em>。</td>
      </tr>
    </tbody>
  </table>

5. 將下列程式碼新增至 AppDelegate 檔案。
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: pre}

## 步驟 4. 管理登入經驗
{: #managing-signin-appid}

身分提供者會為您的使用者提供鑑別資訊，以便您授權給他們。使用 {{site.data.keyword.appid_short_notm}}，您可以使用社交身分提供者，例如，Facebook 及 Google+，或者您可以使用雲端目錄來管理使用者登錄。

您可以隨時更新您的配置，而不需要更新應用程式程式碼。
{: tip}


### 社交身分提供者

使用 {{site.data.keyword.appid_short_notm}}，您可以使用社交身分提供者，例如，Facebook 及 Google+，來保護您的應用程式。

若要配置社交身分提供者，請執行下列動作：

1. 開啟 {{site.data.keyword.appid_short_notm}} 儀表板的**身分提供者 > 管理**。
2. 將您要使用的身分提供者設為**開啟**。您可以使用任何組合的身分提供者，但是如果您要帶入自訂的登入畫面，則需要只啟用「雲端目錄」。
3. 將[預設配置](/docs/services/appid/identity-providers.html)更新為您自己的認證。{{site.data.keyword.appid_short_notm}} 提供 IBM 認證，您可以用來試用服務，但在發佈您的應用程式之前，需要更新配置。
4. 自訂預先配置的登入畫面，以顯示您選擇的影像及顏色。
5. 若要使用您的應用程式來呼叫登入小組件，請將下列指令新增至您的程式碼。
    ```swift
    import BluemixAppID
    class delegate : AuthorizationDelegate {
        public func onAuthorizationSuccess(accessToken: AccessToken, identityToken: IdentityToken, response:Response?) {
            //User authenticated
        }

        public func onAuthorizationCanceled() {
            //Authentication canceled by the user
        }

        public func onAuthorizationFailure(error: AuthorizationError) {
            //Exception occurred
        }
    }

    AppID.sharedInstance.loginWidget?.launch(delegate: delegate())
    ```
    {: codeblock}


### 雲端目錄

使用 {{site.data.keyword.appid_short_notm}}，您可以管理您自己的使用者登錄，稱為雲端目錄。雲端目錄可讓使用者利用其電子郵件及密碼，來註冊並登入您的行動及 Web 應用程式。

若要帶入您自創品牌的使用者介面畫面，只有雲端目錄可以作為身分提供者。
{: tip}

若要配置雲端目錄，請執行下列動作：

1. 開啟 {{site.data.keyword.appid_short_notm}} 儀表板的**管理身分提供者**標籤，然後將雲端目錄設為**開啟**。
2. 配置您的[目錄及訊息設定](/docs/services/appid/cloud-drectory.html)。
4. 選擇您要顯示的登入畫面組合，並在您的應用程式中放置程式碼以呼叫那些畫面。
    * 登入
        ```swift
        class delegate : TokenResponseDelegate {
            public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
            //User authenticated
            }

            public func onAuthorizationFailure(error: AuthorizationError) {
            //Exception occurred
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
              //email verification is required
              return
             }
           //User authenticated
          }

          public func onAuthorizationCanceled() {
              //Sign up canceled by the user
          }

          public func onAuthorizationFailure(error: AuthorizationError) {
              //Exception occurred
          }
        }

        AppID.sharedInstance.loginWidget?.launchSignUp(delegate: delegate())
        ```
        {: codeblock}

    * 忘記密碼
        ```swift
        class delegate : AuthorizationDelegate {
           public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
              //forgot password finished, in this case accessToken and identityToken will be null.
           }

           public func onAuthorizationCanceled() {
               //forgot password canceled by the user
           }

           public func onAuthorizationFailure(error: AuthorizationError) {
               //Exception occurred
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
{: #appid_testing}

所有項目都正確設定嗎？您可以測試看看！

1. 以 Xcode 模擬器開啟您的應用程式。
2. 使用 GUI，逐步進行登入應用程式的處理程序。如果您已配置雲端目錄，請確定所有畫面的顯示都如您所願。
3. 更新 {{site.data.keyword.appid_short_notm}} 儀表板中的身分提供者或登入小組件畫面。
4. 重複步驟 1 和 2，查看是否立即實作變更。不需要更新您的應用程式程式碼。

有困難嗎？請查看[疑難排解 {{site.data.keyword.appid_short_notm}}](/docs/services/appid/ts_index.html)。

## 後續步驟
{: #appid_next}

做得好！您已新增一個安全等級至您的應用程式。嘗試下列其中一個選項，以保持動力：

* 進一步瞭解並充分利用 {{site.data.keyword.appid_short_notm}} 所提供的所有特性，[請查看文件](/docs/services/appid/index.html)！
* 「入門範本套件」是利用 IBM Cloud 功能最快的方式之一。請檢視[行動開發人員儀表板](https://console.bluemix.net/developer/mobile/dashboard)中的可用入門範本套件。下載程式碼。執行應用程式！
