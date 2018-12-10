---

copyright:
  years: 2018
lastupdated: "2018-08-07"

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
{: #before-you-begin}

다음 전제조건을 제대로 갖추었는지 확인하십시오.

  * [{{site.data.keyword.conversationshort}} 서비스](/docs/services/conversation/getting-started.html)의 인스턴스
  * iOS 8.0+
  * Xcode 9.0+
  * Swift 3.2+ 또는 Swift 4.0+
  * Carthage

[Carthage](https://github.com/Carthage/Carthage){:new_window}를 사용하여 종속성을 관리하고 사용자의 애플리케이션에 적합한 {{site.data.keyword.watson}} Swift SDK를 빌드합니다. Carthage를 처음 사용하는 경우 [Homebrew](http://brew.sh/){:new_window}를 사용하여 설치할 수 있습니다.

```bash
$ brew update
$ brew install carthage
```

## 종속성 다운로드 및 빌드
{: #download-and-build-dependencies}

1. 선호하는 텍스트 편집기를 사용하여 프로젝트의 루트 디렉토리(여기에 `.xcodeproj` 파일이 있음)에 `Cartfile`이라는 새 파일을 작성하십시오.

2. 종속성으로 Watson Swift SDK를 지정하는 행을 추가하십시오.
  ```
  github "watson-developer-cloud/swift-sdk"
  ```
  {: codeblock}

  프로덕션 앱의 경우 {{site.data.keyword.watson}} Swift SDK의 새 릴리스에서 발생하는 예상치 못한 변경사항을 방지하려면 [버전 요구사항](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement){:new_window}을 지정할 수 있습니다.
  {: tip}

3. 터미널을 사용하여 프로젝트의 루트 디렉토리로 이동한 후 Carthage를 실행하십시오.
  ```bash
  carthage update --platform iOS
  ```
  {: codeblock}

  그러면 {{site.data.keyword.watson}} Swift SDK가 다운로드되고 프레임워크가 프로젝트의 `Carthage/Build/iOS` 폴더에서 빌드됩니다.

## 엡에 프레임워크 추가
{: #add-frameworks-to-your-app}

{{site.data.keyword.watson}} Swift SDK 프레임워크가 빌드되었으므로 연결하여 {{site.data.keyword.conversationshort}} 프레임워크를 앱에 복사하십시오.

1. Xcode에서 앱을 열고 네비게이터에서 프로젝트를 선택하여 설정을 여십시오.
2. 앱 대상을 선택하고 **일반** 탭을 여십시오.
3. 연결된 프레임워크 및 라이브러리 섹션으로 스크롤하고 `+` 아이콘을 클릭하십시오.
4. 표시되는 새 창에서 **기타 항목 추가**를 클릭하고 `Carthage/Build/iOS` 디렉토리로 이동하십시오.
5. `AssistantV1.framework`를 선택하여 앱을 통해 연결하십시오.

{{site.data.keyword.conversationshort}} 프레임워크에 연결하는 것 외에도 이 프레임워크를 앱에 복사하여 런타임 시 액세스할 수 있게 해야 합니다. 그런 다음 Carthage 스크립트가 특정한 [앱 스토어 제출 버그](http://www.openradar.me/radar?id=6409498411401216){:new_window}를 방지하기 위해 사용됩니다.

1. Xcode에 앱 대상의 설정이 열려 있는 상태에서 **빌드 단계** 탭으로 이동하십시오.
2. `+` 아이콘을 클릭하여 **새 실행 스크립트 단계**를 선택하십시오.
3. `/usr/local/bin/carthage copy-frameworks` 명령을 실행 스크립트 단계에 추가하십시오.
4. {{site.data.keyword.conversationshort}} 프레임워크를 **입력 파일** 목록에 추가하십시오.
  ```
  $(SRCROOT)/Carthage/Build/iOS/AssistantV1.framework
  ```
  {: codeblock}

## 가상 어시스턴트를 앱에 추가하십시오.
{: #add-a-virtual-assistant-to-your-app}

1. Xcode에서 `ViewController.swift`를 여십시오.
2. {{site.data.keyword.conversationshort}}용 import 문을 추가하십시오(예: `import AssistantV1`).
3. `assistantExample`이라고 하는 비어 있는 함수를 작성한 후 `viewDidLoad`에서 이를 호출하십시오.
4. `assistantExample` 함수에 대한 다음 코드를 추가하십시오. 사용자 이름, 비밀번호 및 작업공간 ID를 업데이트하십시오.

```swift
//
//  ViewController.swift
//  WatsonAssistantExample
//
//  Created by Glenn R. Fisher on 3/1/18.
//  Copyright © 2018 Glenn R. Fisher. All rights reserved.
//

import UIKit
import AssistantV1

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        assistantExample()
    }

    func assistantExample() {
        // Assistant credentials
        let username = "your-username-here"
        let password = "your-password-here"
        let workspace = "your-workspace-id-here"

        // instantiate service
        let assistant = Assistant(username: username, password: password, version: "2018-03-01")

        // start a conversation
        assistant.message(workspaceID: workspace) { response in
            print("Conversation ID: \(response.context.conversationID!)")
            print("Response: \(response.output.text.joined())")

            // continue assistant
            print("Request: turn the radio on")
            let input = InputData(text: "turn the radio on")
            let request = MessageRequest(input: input, context: response.context)
            assistant.message(workspaceID: workspace, request: request) { response in
                print("Response: \(response.output.text.joined())")
            }
        }
    }
}
```
{: codeblock}

앱을 실행하면 콘솔에 다음 메시지가 표시됩니다.
```
Conversation ID: cbb18524-1e78-4bb5-a6ea-ceb9311da391
Response: Hi. It looks like a nice drive today. What would you like me to do?
Request: turn the radio on
Response: Sure thing! Which genre would you prefer? Jazz is my personal favorite..
```
{: screen}

## 스타터 킷 사용
{: #conversation_starterkits}

스터터 킷을 사용하면 {{site.data.keyword.cloud_notm}}의 기능을 빠르고 쉽게 활용할 수 있습니다. 스타터 킷을 사용하여 {{site.data.keyword.conversationshort}}을 서버 측 백엔드에 추가할 수 있습니다. Chatbot for iOS with Watson 스타터 킷은 애플리케이션에 일반 사용자와의 상호작용을 자동화하는 자연어 인터페이스를 추가하여 {{site.data.keyword.conversationshort}}의 심화 학습 기능을 사용하는 방법을 보여줍니다.

1. 작업할 [스타터 킷](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window}을 선택하십시오.
2. 기본 서비스를 사용하여 프로젝트를 작성하십시오.
3. **리소스 추가 > Watson > {{site.data.keyword.conversationshort}}**을 클릭하십시오.
4. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. `config/local-dev.json` 파일에서 서비스 인증 정보를 찾을 수 있습니다.

## 다음 단계
{: #assistant_next}

잘 하셨습니다! 앱에 AI 어시스턴트를 추가했습니다. 다음 옵션 중 하나를 사용하여 계속 진행하십시오.

* [{{site.data.keyword.watson}} Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){:new_window}를 확인하십시오.
* [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html)에서 제공하는 모든 기능을 활용하십시오.
* GitHub에서 {{site.data.keyword.watson}} Swift SDK를 시연하는 [Simple Chat 샘플 앱](https://github.com/watson-developer-cloud/simple-chat-swift){:new_window}의 소스 코드를 보십시오.
