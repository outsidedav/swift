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

# 사용자 인증 추가
{: #appid}

애플리케이션 보안은 상당히 복잡합니다. 대부분의 개발자에게 애플리케이션 보안은 앱을 작성하는 데 좀 더 어려운 태스크 중 하나입니다. 사용자 정보를 보호하고 있음을 어떻게 확신합니까? {{site.data.keyword.appid_full}}를 앱과 통합하면 보안 분야에 대한 경험이 많지 않더라도 리소스를 보호하고 인증을 추가할 수 있습니다.

사용자가 로그인하도록 요청하여 앱 환경 설정과 같은 사용자 데이터(또는 공용 소셜 프로파일의 정보)를 저장할 수 있고, 이후에 해당 데이터를 사용하여 앱 내에서 각 사용자 경험을 사용자 정의할 수 있습니다. {{site.data.keyword.appid_short_notm}}는 사용자를 위한 로그인 프레임워크를 제공하지만 클라우드 디렉토리에 사용할 브랜딩된 고유한 로그인 화면을 가져올 수도 있습니다.

{{site.data.keyword.appid_short_notm}} 및 아키텍처 정보를 사용할 수 있는 모든 방법은 [{{site.data.keyword.appid_short_notm}} 정보](/docs/services/appid?topic=appid-about#about)를 참조하십시오.

## 시작하기 전에
{: #prereqs-appid}

먼저, 다음 필수 소프트웨어를 갖추었는지 확인하십시오.
* CocoaPods(버전 1.1.0 이상)
* iOS(버전 9 이상)
* MacOS(버전 10.11.5 이상)
* Xcode(버전 9.0.1 이상)

## 1단계. {{site.data.keyword.appid_short_notm}}의 인스턴스 작성
{: #create-instance-appid}

{{site.data.keyword.appid_short_notm}} 서비스의 인스턴스를 작성하십시오.

1. [{{site.data.keyword.cloud_notm}} 카탈로그](https://{DomainName}/catalog){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")에서 {{site.data.keyword.appid_short_notm}}를 선택하십시오. 서비스 구성 화면이 열립니다.
2. 서비스 인스턴스에 이름을 지정하거나 사전 설정된 이름을 사용하십시오.
3. 가격 플랜을 선택하고 **작성**을 클릭하십시오.

## 2단계. iOS Swift SDK 설치
{: #install-sdk-appid}

서비스는 앱을 보다 쉽게 코딩하는 데 도움이 되는 SDK를 제공합니다. SDK가 앱 코드에 설치되어야 합니다.

1. `Podfile`에 대한 기존 Xcode 프로젝트 디렉토리를 여십시오.

2. 프로젝트 대상에서 `IBMCloudAppID` 팟(Pod)의 종속성을 추가하십시오. 다음 예에서 표시된 대로 `use_frameworks!` 명령도 대상 아래에 있는지 확인하십시오.
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'IBMCloudAppID'
    end
    ```
    {: pre}

3. `IBMCloudAppID` 종속성을 다운로드하십시오.
    ```
    pod install --repo-update
    ```
    {: pre}

## 3단계. SDK 초기화
{: #initialize-sdk-appid}

앱에서 SDK를 초기화한 후 {{site.data.keyword.appid_short_notm}} 환경 설정의 구성을 시작할 수 있습니다.

1. **프로젝트 설정 > 기능 > 키 체인 공유**로 이동하여 Xcode 프로젝트에서 키 체인 공유를 사용으로 설정하십시오.

2. **프로젝트 설정 > 정보 > URL 유형**으로 이동하여 다음 값을 **URL 스키마** 및 **ID** 텍스트 상자 모두에 추가하십시오.
  ```
    $(PRODUCT_BUNDLE_IDENTIFIER)
  ```
  {: codeblock}

3. 다음 가져오기를 `AppDelegate.swift` 파일에 추가하십시오.
  ```swift
  import IBMCloudAppID
  ```
  {: codeblock}

4. `tenantID` 및 `AppID_region` 매개변수를 전달하여 SDK를 초기화하십시오. 사용자의 앱에서 공통이지만 필수는 아닌 코드를 배치할 위치는 `AppDelegate`의 `application:didFinishLaunchingWithOptions` 메소드에 있습니다.
  ```swift
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, <AppID_region>);
  ```
  {: codeblock}
  
  * `tenantID`: 테넌트 ID는 앱을 초기화하는 데 사용되는 고유 ID입니다. **서비스 인증 정보** 탭을 선택하여 {{site.data.keyword.appid_short_notm}} 대시보드에서 값을 찾은 후 **인증 정보 보기**를 클릭할 수 있습니다.
  * `AppID_region`: {{site.data.keyword.appid_short_notm}} 지역은 서비스에 대해 작업하는 {{site.data.keyword.cloud_notm}} 지역입니다. 이 지역은 서비스 대시보드에서 찾을 수 있으며 `AppID.REGION_US_SOUTH`, `AppID.REGION_SYDNEY` 및 `AppID.REGION_UK`가 될 수 있습니다.

5. 다음 코드를 `AppDelegate` 파일에 추가하십시오.
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: codeblock}

## 4단계. 로그인 경험 관리
{: #managing-signin-appid}

ID 제공자는 사용자에게 권한을 부여할 수 있도록 사용자에 대한 인증 정보를 제공합니다. {{site.data.keyword.appid_short_notm}}를 사용하면 Facebook 및 Google+와 같은 소셜 ID 제공자를 사용할 수 있으며, 클라우드 디렉토리를 사용하여 사용자 레지스트리를 관리할 수 있습니다.

앱 코드를 업데이트하지 않고 언제든지 구성을 업데이트할 수 있습니다.
{: tip}

### 소셜 ID 제공자
{: #social-appid}

{{site.data.keyword.appid_short_notm}}를 통해 소셜 ID 제공자(예: Facebook 및 Google+)를 사용하여 앱을 보호할 수 있습니다.

소셜 ID 제공자를 구성하려면 다음을 수행하십시오.

1. {{site.data.keyword.appid_short_notm}} 대시보드를 열고 **ID 제공자 > 관리**로 이동하십시오.
2. 사용할 ID 제공자를 **켜짐**으로 설정하십시오. ID 제공자의 모든 조합을 사용할 수 있으나 사용자 정의된 사인온 화면을 가져오려면 클라우드 디렉토리만 사용으로 설정해야 합니다.
3. 자신의 인증 정보로 [기본 구성](/docs/services/appid?topic=appid-social#social)을 업데이트하십시오. {{site.data.keyword.appid_short_notm}}는 서비스를 실행해 보는 데 사용할 수 있는 IBM 인증 정보를 제공하지만 앱을 공개하기 전에 구성을 업데이트해야 합니다.
4. 원하는 이미지와 색상을 표시하려면 로그인 화면을 사용자 정의하십시오.
5. 앱을 사용하여 로그인 위젯을 호출하려면 다음 명령을 코드에 추가하십시오.
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

### 클라우드 디렉토리
{: #cloud-dir-appid}

{{site.data.keyword.appid_short_notm}}를 사용하면 클라우드 디렉토리라는 고유 사용자 레지스트리를 관리할 수 있습니다. 클라우드 디렉토리는 사용자가 자신의 이메일 및 비밀번호를 사용하여 모바일 및 웹 앱에 가입하고 로그인할 수 있게 해 줍니다.

브랜딩된 고유한 UI 화면을 가져오려면 클라우드 디렉토리만 ID 제공자로 사용할 수 있습니다.
{: tip}

클라우드 디렉토리를 구성하려면 다음을 수행하십시오.

1. {{site.data.keyword.appid_short_notm}} 대시보드를 열어 **ID 제공자 관리** 탭으로 이동한 후 클라우드 디렉토리를 **켜짐**으로 설정하십시오.
2. [디렉토리 및 메시지 설정](/docs/services/appid?topic=appid-cloud-directory)을 구성하십시오.
4. 표시할 사인온 화면의 조합을 선택하고 애플리케이션에서 해당 화면을 호출하도록 코드를 배치하십시오.
    * 로그인
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

    * 가입
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

    * 비밀번호 찾기
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

    * 계정 세부사항
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

    * 비밀번호 변경
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


## 5단계. 앱 테스트
{: #testing-appid}

모든 항목이 올바르게 구성되어 있습니까? 이는 테스트를 통해 확인할 수 있습니다.

1. Xcode 에뮬레이터로 앱을 여십시오.
2. GUI를 사용하여 애플리케이션 로그인 프로세스를 진행해 보십시오. 클라우드 디렉토리를 구성한 경우 모든 화면이 의도한 대로 표시되고 있는지 확인하십시오.
3. {{site.data.keyword.appid_short_notm}} 대시보드에서 ID 제공자 또는 로그인 위젯 화면을 업데이트하십시오.
4. 1단계 및 2단계를 반복하여 변경사항이 즉시 구현되는지 확인하십시오. 앱 코드를 업데이트할 필요는 없습니다.

문제가 있습니까? [{{site.data.keyword.appid_short_notm}} 문제점 해결](/docs/services/appid?topic=appid-troubleshooting)을 참조하십시오.

## 다음 단계
{: #next-appid notoc}

잘 하셨습니다! 앱에 보안 레벨을 추가했습니다. 다음 옵션 중 하나를 사용하여 계속 진행하십시오.

* {{site.data.keyword.appid_short_notm}}에서 제공하는 모든 기능을 자세히 알아보고 활용하십시오. [이 문서를 확인하십시오](/docs/services/appid?topic=appid-getting-started#getting-started).
* 스타터 킷은 {{site.data.keyword.cloud_notm}}의 기능을 사용할 수 있는 가장 빠른 방법 중 하나입니다. [모바일 개발자 대시보드](https://{DomainName}/developer/mobile/dashboard){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")에서 사용 가능한 스타터 킷을 보십시오. 코드를 다운로드하십시오. 앱을 실행하십시오!
