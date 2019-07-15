---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: chatbot swift, virtual assistant swift, assistant swift, watson swift starter, assistantv2 swift, watson sdk swift, add chatbot swift, add assistant swift

subcollection: swift

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 챗봇 추가
{: #assistant}

{{site.data.keyword.conversationshort}} 서비스를 사용하여 자연어 입력에 응답하고 인간과 같은 대화를 통해 사용자에게 응답하는 애플리케이션을 빌드할 수 있습니다.

다음 목록은 통합이 수행되는 방법의 플로우에 대해 간략하게 설명합니다.

1. 사용자는 앱에서 구현되는 인터페이스와 상호작용합니다.
2. 앱은 {{site.data.keyword.watson}} Swift SDK를 사용하여 사용자 입력을 {{site.data.keyword.conversationshort}}에게 전송합니다.
3. {{site.data.keyword.watson}} Swift SDK는 대화 상자 플로우 및 훈련 데이터를 위한 컨테이너인 작업공간에 연결합니다.
4. 작업공간에서는 사용자 입력을 해석하고 대화의 플로우를 지시하며, 애플리케이션에 대한 응답을 전송합니다.
5. 앱은 사용자의 응답을 표시합니다.

## 시작하기 전에
{: #prereqs-chatbot}

다음 전제조건을 갖추었는지 확인하십시오.

* [{{site.data.keyword.conversationshort}} 서비스](/docs/services/assistant?topic=assistant-getting-started#getting-started)의 인스턴스
* iOS 10.0+
* Xcode 9.3+
* Swift 4.1+
* CocoaPods, Carthage 또는 Swift Package Manager

[CocoaPods](https://github.com/watson-developer-cloud/swift-sdk#cocoapods){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘"), [Carthage](https://github.com/watson-developer-cloud/swift-sdk#carthage){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 또는 [Swift Package Manager](https://github.com/watson-developer-cloud/swift-sdk#swift-package-manager){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 사용하여 [Watson Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 설치할 수 있습니다. 종속성을 관리하는 데 CocoaPods를 사용하면, 전체 Watson Swift SDK와는 반대로 필요한 프레임워크만 가져옵니다. CocoaPods를 처음 사용하는 경우 다음으로 이를 쉽게 설치할 수 있습니다.

```console
  sudo gem install cocoapods
```
{: codeblock}

## 1단계. Watson Assistant의 인스턴스 작성
{: #instance-watson-chatbot}

{{site.data.keyword.conversationshort}} 서비스의 인스턴스를 프로비저닝하십시오.

1. {{site.data.keyword.cloud_notm}} 카탈로그에서 **{{site.data.keyword.conversationshort}}**를 클릭하십시오. 서비스 구성 화면이 열립니다.
2. 서비스 인스턴스에 이름을 지정하거나 사전 설정된 이름을 사용하십시오.
3. 인스턴스를 앱에 바인드하려면 **연결** 메뉴에서 앱을 선택하십시오.
4. 가격 플랜을 선택하고 **작성**을 클릭하십시오.
5. **인증 정보** 탭을 선택하여 서비스 인증 정보를 확인하십시오. 이 값은 앱에서 서비스를 연결하는 데 사용됩니다.
6. **도구 실행**을 클릭하여 챗봇 어시스턴트를 빌드하십시오. 랜딩 페이지의 지시사항을 따라 고유한 챗봇을 빌드하거나 **스킬** 탭으로 이동하고 **새로 작성**을 선택하십시오. **대화 상자 스킬 추가** 페이지에서 **샘플 스킬 사용** 탭을 선택하고 **고객 지원 샘플 스킬**을 선택하여 빠르게 시작하십시오. 계속해서 이 스킬을 세분화하거나 나중에 앱에서 사용될 다른 스킬을 작성할 수 있습니다.
7. 새 스킬의 메뉴를 클릭하고 **API 세부사항 보기**를 선택하십시오. 이제 서비스 인증 정보 외에 필요한 `Workspace ID`가 표시됩니다.

## 2단계. 종속성 다운로드 및 빌드
{: #download-depend-chatbot}

다음 예에서는 AssistantV1을 사용합니다. AssistantV2 프레임워크에 대한 자세한 정보는 [Watson SDK AssistantV2 문서](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV2/index.html){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.

선호하는 텍스트 편집기를 사용하여 `pod init`의 실행을 통해 프로젝트의 루트 디렉토리(여기에 `.xcodeproj` 파일이 있음)에 새 `Podfile`을 작성하십시오. 그런 다음 종속성으로 Watson Swift SDK의 {{site.data.keyword.conversationshort}} 프레임워크를 지정하는 행을 추가하십시오.

```pod
use_frameworks!

target 'MyApp' do
    pod 'IBMWatsonAssistantV1'
```
{: codeblock}

프로덕션 앱의 경우 새 Watson Swift SDK의 새 릴리스에서 발생하는 예상치 못한 변경사항을 방지하기 위해 [버전 요구사항](https://guides.cocoapods.org/using/the-podfile.html#specifying-pod-versions){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 지정할 수도 있습니다.

`Podfile`이 올바르게 배치되면 이제 종속성을 다운로드할 수 있습니다. 터미널을 사용하여 프로젝트의 루트 디렉토리로 이동한 후 CocoaPods를 실행하십시오.

```console
  pod install
```
{: codeblock}

Cocoapods는 {{site.data.keyword.conversationshort}} 프레임워크를 다운로드하고 프로젝트의 `Pods/` 폴더에 이를 빌드합니다.

Pod 빌드 실패를 방지하려면 Xcode에서 프로젝트를 열 때 `.xcodeproj` 대신 `.xcworkspace`로 끝나는 파일을 여십시오.
{: tip}

## 3단계. 앱에 가상 어시스턴트 추가
{: #virtual-assist-chatbot}

다음 샘플은 일반적으로 `ViewController.swift`에서 {{site.data.keyword.conversationshort}} 기능을 사용자의 애플리케이션에 추가하는 것을 도와줍니다. 다음 샘플을 사용하여 사용자의 유스 케이스에 적합한 어시스턴트 호출을 확장할 수 있습니다.

1. {{site.data.keyword.conversationshort}}용 import 문을 추가하십시오(예: `import Assistant`).
  ```swift
  import Assistant
  ```
  {: codeblock}

2. {{site.data.keyword.conversationshort}} 서비스를 인스턴스화하십시오.
  ```swift
  let assistant = Assistant(version: "yyyy-mm-dd", apikey: "your-api-key-here")

  // save context to state to continue the conversation later
  var context: Context?
  ```
  {: codeblock}

  **팁**: 이 예에서는 표시할 컨텍스트를 저장합니다. 이 오브젝트에 대한 이해를 돕고 사용자의 유스 케이스에 맞게 조정하는 방법을 보려면 [컨텍스트 변수 문서](/docs/services/assistant?topic=assistant-dialog-runtime#context-variables)를 참조하십시오. [버전 매개변수 문서](https://{DomainName}/apidocs/assistant#versioning){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 확인하거나 {site.data.keyword.conversationshort}} 서비스가 작성된 날짜를 사용하십시오.

3. 대화를 초기화하십시오. 어시스턴트 구성 방법에 따라 다음과 같이 사용자에게 초기 응답을 제공할 수 있습니다.
  ```swift
  // Start a conversation
  assistant.message(workspaceID: "your-workspace-ID-here) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Set the context to state
      self.context = message.context    
  }
  ```
  {: codeblock}

4. 메시지와 컨텍스트를 어시스턴트에 전송하십시오.
  ```swift
  print("Request: When are you open?")
  let input = InputData(text: "When are you open?")

  assistant.message(workspaceID: workspace, input: input, context: self.context) { response, error in
      if let error = error {
         print(error)
    }

      guard let message = response?.result else {
          print("Failed to get the message.")
          return
      }

      print("Conversation ID: \(message.context.conversationID!)")
      print("Response: \(message.output.text.joined())")

      // Update the context
      self.context = message.context
  }
  ```
  {: codeblock}

기본 어시스턴트를 사용하여 위의 예로 앱을 실행하면 콘솔에 다음 메시지가 표시됩니다.
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hello, I’m a demo customer care virtual assistant to show you the basics.  I can help with directions to my store, hours of operation and booking an in-store appointment.
Request: When are you open?
Response: Our hours are Monday to Friday 10am to 8pm and Friday and Saturday 11am to 6pm.
```
{: screen}

5. Watson SDK [Assistant 문서](https://watson-developer-cloud.github.io/swift-sdk/services/AssistantV1/index.html){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 탐색하여 애플리케이션의 기능을 빌드하십시오.

## 스타터 킷 사용
{: #starterkits-chatbot}

스터터 킷을 사용하면 {{site.data.keyword.cloud_notm}}의 기능을 빠르고 쉽게 이용할 수 있습니다. 스타터 킷을 사용하여 {{site.data.keyword.conversationshort}}을 서버 측 백엔드에 추가할 수 있습니다. Chatbot for iOS with Watson 스타터 킷은 애플리케이션에 사용자와의 상호작용을 자동화하는 자연어 인터페이스를 추가하여 {{site.data.keyword.conversationshort}}의 심화 학습 기능을 사용하는 방법을 보여줍니다.

1. 작업할 [스타터 킷](https://{DomainName}/developer/appledevelopment/starter-kits){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")을 선택하십시오.
2. 기본 서비스를 사용하여 앱을 작성하십시오.
3. **서비스 추가 > Watson > {{site.data.keyword.conversationshort}}**을 클릭하십시오.
4. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. `config/local-dev.json` 파일에서 서비스 인증 정보를 찾을 수 있습니다.

## 다음 단계
{: #next-chatbot notoc}

잘 하셨습니다! 앱에 AI 어시스턴트를 추가했습니다. 다음 옵션 중 하나를 사용하여 계속 진행하십시오.

* [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 확인하고 기타 지원되는 Watson 서비스를 탐색하십시오.
* [{{site.data.keyword.conversationshort}}](/docs/services/assistant?topic=assistant-index#index)에서 제공하는 모든 기능을 활용하십시오.
* GitHub에서 {{site.data.keyword.watson}} Swift SDK를 시연하는 [Simple Chat 샘플 앱](https://github.com/watson-developer-cloud/simple-chat-swift){: new_window} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")의 소스 코드를 보십시오.
