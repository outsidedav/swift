---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# ユーザー認証の追加

アプリケーション・セキュリティーは非常に複雑です。ほとんどの開発者にとって、アプリ作成における難題の 1 つとなっています。確実にユーザー情報を保護するにはどうすればよいでしょうか? アプリに {{site.data.keyword.appid_full}} を統合することによって、セキュリティーに関する経験があまりなくても、リソースを保護し、認証を追加することができます。

ユーザーにサインインを要求することで、アプリ設定などのユーザー・データ (または公開されているソーシャル・プロファイル情報) を保管し、そのデータを使用してアプリ内のエクスペリエンスをユーザーごとにカスタマイズできます。 {{site.data.keyword.appid_short_notm}} により手軽なログイン・フレームワークが提供されていますが、クラウド・ディレクトリーで使用するために、独自のブランド・マークが付いたサインイン画面を表示させることもできます。

{{site.data.keyword.appid_short_notm}} の詳しい使用方法とアーキテクチャー情報については、[{{site.data.keyword.appid_short_notm}} について](/docs/services/appid/about.html)を参照してください。

## 始めに
{: #before}

まず、以下の前提条件が整っていることを確認してください。
* CocoaPods (バージョン 1.1.0 以上)
* iOS (バージョン 9 以上)
* MacOS (バージョン 10.11.5 以上)
* Xcode (バージョン 9.0.1 以上)

## ステップ 1. {{site.data.keyword.appid_short_notm}} のインスタンスの作成
{: #create_instance}

{{site.data.keyword.appid_short_notm}} サービスのインスタンスを次のように作成します。

1. [{{site.data.keyword.cloud_notm}} カタログ](https://console.bluemix.net/catalog/)で、{{site.data.keyword.appid_short_notm}} を選択します。 サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. 料金プランを選択し、**「作成」**をクリックします。

## ステップ 2. iOS Swift SDK のインストール
{: #install_sdk}

このサービスで提供されている SDK を使用すると、アプリのコーディングがより簡単になります。 この SDK は、アプリ・コードにインストールする必要があります。

1. 既存の Xcode プロジェクト・ディレクトリーを開き、`Podfile` に移動します。

2. プロジェクト・ターゲットの下で、`BluemixAppID` ポッドの従属関係を追加します。 また、次の例のように `use_frameworks!` コマンドがターゲットの下に存在することも確認してください。
    ```swift
    target '<yourTarget>' do
      use_frameworks!
        pod 'BluemixAppID'
    end
    ```
    {: pre}

3. `BluemixAppID` 従属関係をダウンロードします。
    ```
    pod install --repo-update
    ```
    {: pre}

## ステップ 3. SDK の初期化

アプリで SDK を初期化した後、{{site.data.keyword.appid_short_notm}} の設定を開始できます。

1. **「プロジェクト設定」>「機能」>「キーチェーン共有 (Keychain sharing)」**に進み、Xcode プロジェクトでのキーチェーン共有を有効にします。

2. **「プロジェクト設定」>「情報」>「URL タイプ」**に進み、**「URL スキーム」**および**「ID」**の両方のテキスト・ボックスに以下の値を追加します。
  ```
  $(PRODUCT_BUNDLE_IDENTIFIER)
  ```
  {: codeblock}

3. `AppDelegate.swift` ファイルに以下のインポートを追加します。
  ```swift
  import BluemixAppID
  ```
  {: codeblock}

4. `tenant ID` および `region` パラメーターを渡して、SDK を初期化します。このコードを入れる一般的な場所 (ただし必須の場所ではありません) は、アプリの `AppDelegate` の `application:didFinishLaunchingWithOptions` メソッド内です。
  ```swift
  AppID.sharedInstance.initialize(tenantId: <tenantId>, bluemixRegion: <AppID_region>)
  ```
  {: codeblock}
  
  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt=""/> コマンドの構成要素の説明 </th>
    </thead>
    <tbody>
      <tr>
        <td><em>tenantID</em></td>
        <td>テナント ID は、アプリの初期化に使用される固有 ID です。 この値は {{site.data.keyword.appid_short_notm}} ダッシュボードで確認することができます。 <b>「サービス資格情報 (Service Credentials)」</b>タブの<b>「資格情報の表示 (View Credentials)」</b>をクリックします。</td>
      </tr>
      <tr>
        <td><em>AppID_region</em></td>
        <td>{{site.data.keyword.appid_short_notm}} リージョンは、サービスの操作場所の {{site.data.keyword.cloud_notm}} リージョンです。 このリージョンはサービス・ダッシュボードにあり、<em>AppID.REGION_US_SOUTH</em>、<em>AppID.REGION_SYDNEY</em>、<em>AppID.REGION_UK</em> のいずれかになります。</td>
      </tr>
    </tbody>
  </table>

5. 以下のコードを AppDelegate ファイルに追加します。
    ```swift
    func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
            return AppID.sharedInstance.application(application, open: url, options: options)
        }
    ```
    {: codeblock}

## ステップ 4. サインイン・エクスペリエンスの管理
{: #managing-signin-appid}

ID プロバイダーが提供するユーザー認証情報を使用して、ユーザーに権限を与えることができます。 {{site.data.keyword.appid_short_notm}} では、Facebook や Google+ などのソーシャル ID プロバイダーを使用することも、クラウド・ディレクトリーでユーザー・レジストリーを管理することもできます。

アプリ・コードを更新せずに、いつでも構成を更新できます。
{: tip}


### ソーシャル ID プロバイダー

{{site.data.keyword.appid_short_notm}} では、Facebook や Google+ などのソーシャル ID プロバイダーを使ってアプリを保護することができます。

ソーシャル ID プロバイダーを構成するには、次のようにします。

1. {{site.data.keyword.appid_short_notm}} ダッシュボードを開いて**「ID プロバイダー」>「管理」**に移動します。
2. 使用する ID プロバイダーを**「オン」**に設定します。 複数の ID プロバイダーを任意に組み合わせることができますが、サインオン画面をカスタマイズするには、クラウド・ディレクトリーのみを有効にする必要があります。
3. [デフォルト構成](/docs/services/appid/identity-providers.html)を実際の資格情報に更新します。 {{site.data.keyword.appid_short_notm}} にある IBM 資格情報を使ってサービスを試してみることもできますが、アプリを公開する前に構成を更新する必要があります。
4. 自分で選択したイメージと色が表示されるように、サインイン画面をカスタマイズします。
5. アプリでログイン・ウィジェットを呼び出すには、次のコマンドをコードに追加します。
    ```swift
    import BluemixAppID
    class delegate : AuthorizationDelegate {
        public func onAuthorizationSuccess(accessToken: AccessToken, identityToken: IdentityToken, response:Response?) {
            //ユーザー認証済み
        }

        public func onAuthorizationCanceled() {
            //ユーザーによる認証の取り消し
        }

        public func onAuthorizationFailure(error: AuthorizationError) {
            //例外の発生
        }
    }

    AppID.sharedInstance.loginWidget?.launch(delegate: delegate())
    ```
    {: codeblock}


### クラウド・ディレクトリー

{{site.data.keyword.appid_short_notm}} では、クラウド・ディレクトリーという独自のユーザー・レジストリーを管理できます。 クラウド・ディレクトリーを使用すると、ユーザーは自分の E メールとパスワードを使用してモバイル・アプリや Web アプリにサインアップ/サインインできます。

独自のブランド・マークが付いた UI 画面を表示する場合、ID プロバイダーとして有効にできるのはクラウド・ディレクトリーのみです。
{: tip}

クラウド・ディレクトリーを構成するには、次のようにします。

1. {{site.data.keyword.appid_short_notm}} ダッシュボードを開き、**「ID プロバイダーの管理」**タブに移動して、クラウド・ディレクトリーを**「オン」**に設定します。
2. [ディレクトリーおよびメッセージの設定](/docs/services/appid/cloud-drectory.html)を行います。
4. 表示するサインオン画面の組み合わせを選択し、それらの画面を呼び出すコードをアプリケーション内に配置します。
    * サインイン
        ```swift
        class delegate : TokenResponseDelegate {
            public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
            //ユーザー認証済み
            }

            public func onAuthorizationFailure(error: AuthorizationError) {
            //例外の発生
           }
        }

        AppID.sharedInstance.obtainTokensWithROP(username: username, password: password, delegate: delegate())
        ```
        {: codeblock}
    * サインアップ
        ```swift
        class delegate : AuthorizationDelegate {
          public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
             if accessToken == nil && identityToken == nil {
              //E メール検証が必要
              return
             }
           //ユーザー認証済み
          }

          public func onAuthorizationCanceled() {
              //ユーザーによるサインアップの取り消し
          }

          public func onAuthorizationFailure(error: AuthorizationError) {
              //例外の発生
          }
        }

        AppID.sharedInstance.loginWidget?.launchSignUp(delegate: delegate())
        ```
        {: codeblock}

    * パスワードを忘れた場合
        ```swift
        class delegate : AuthorizationDelegate {
           public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
              //パスワード忘れ処理の終了。この場合 accessToken と identityToken はヌル。
           }

           public func onAuthorizationCanceled() {
               //ユーザーによるパスワード忘れ処理の取り消し
           }

           public func onAuthorizationFailure(error: AuthorizationError) {
               //例外の発生
           }
        }

        AppID.sharedInstance.loginWidget?.launchForgotPassword(delegate: delegate())
        ```
        {: codeblock}

    * アカウント詳細
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

    * パスワードの変更
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


## ステップ 5. アプリのテスト
{: #appid_testing}

すべて正しく構成されましたか? テストできます。

1. Xcode エミュレーターでアプリを開きます。
2. GUI を使用してアプリケーションへのサインイン・プロセスを実行します。 クラウド・ディレクトリーを構成した場合は、すべての画面が意図したとおりに表示されることを確認します。
3. {{site.data.keyword.appid_short_notm}} ダッシュボードで ID プロバイダーまたはログイン・ウィジェットの画面を更新します。
4. ステップ 1 と 2 を繰り返して、変更が即時に実装されることを確認します。 アプリ・コードを更新する必要はありません。

問題が発生する場合は、 [{{site.data.keyword.appid_short_notm}} のトラブルシューティング](/docs/services/appid/ts_index.html)を確認してください。

## 次のステップ
{: #appid_next}

お疲れさまでした。 これでアプリに一定のレベルのセキュリティーが追加されました。 この調子で、以下のいずれかのオプションを試してみてください。

* {{site.data.keyword.appid_short_notm}} に備わっているすべての機能を確認して利用するには、[ドキュメント](/docs/services/appid/index.html)をご確認ください。
* スターター・キットは、{{site.data.keyword.cloud_notm}} の機能を素早く使用する方法の 1 つです。 [モバイル開発者ダッシュボード](https://console.bluemix.net/developer/mobile/dashboard)で、使用可能なスターター・キットを確認できます。 コードをダウンロードし、 アプリを実行してください。
