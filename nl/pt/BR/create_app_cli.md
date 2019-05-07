---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-19"

keywords: server-side swift, swift cli, swift dependency, swift commands app, create app swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Criando um app Swift do lado do servidor com a CLI
{: #swift_cli}

O {{site.data.keyword.cloud}} oferece soluções e serviços para permitir aos desenvolvedores do Swift e aos aplicativos segurança, IA e valor exigido por seus clientes. Com um amplo portfólio de ofertas e SDKs, é possível usar esses serviços e trazer seus aplicativos de ponta para o mercado rapidamente.
{: shortdesc}

O guia a seguir é destinado a ajudá-lo a construir, executar localmente e implementar um app Swift do lado do servidor. Saiba como usar o {{site.data.keyword.dev_cli_long}} para executar essas ações com comandos padrão.

É possível usar o {{site.data.keyword.dev_cli_short}} para gerenciar os seus aplicativos do lado do servidor com mais de uma dúzia de comandos. Saiba mais sobre os comandos `ibmcloud dev` na CLI do [{{site.data.keyword.dev_cli_notm}}](/docs/cli/idt?topic=cloud-cli-idt-cli#idt-cli).

## Etapa 1. Requisitos para Desenvolvedores
{: #prereqs-swift-cli}

Para iniciar com o {{site.data.keyword.cloud_notm}}, certifique-se de atender aos requisitos a seguir.

### Sistema Operacional
{: #swift-cli-os-reqs}

Desenvolva apps Swift com a melhor prática usando o hardware suportado mais recente do MacOS e testando com as liberações mais recentes do iOS. Inscreva-se para uma conta do [Apple Developer](https://developer.apple.com/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") para ativar o teste em um dispositivo físico.

### iOS e Xcode
{: #swift-cli-ios_xcode}

- [Instale o Xcode 8+ (ou superior)](https://developer.apple.com/xcode/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
- [Implemente em dispositivos 8 do iOS (ou superiores)](https://support.apple.com/downloads/ios){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
- Antes do envio do app para a Apple, revise as [Diretrizes de envio da App Store](https://developer.apple.com/app-store/guidelines/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")

### SDKs e Gerenciamento de Dependência
{: #swift-cli-sdk-dependency}

As ferramentas a seguir asseguram que você possa instalar os SDKs nativos para trabalhar com os vários {{site.data.keyword.cloud_notm}} Services.

- [Instale o CocoaPods for IBM Cloud SDKs](https://cocoapods.org/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
  ```
  sudo gem install cocoapods
  ```
  {: codeblock}
  
- [Instale o Homebrew para ajudar a instalar o Carthage](https://brew.sh/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
  {: codeblock}

- [Instale o Carthage for Watson SDKs](https://github.com/Carthage/Carthage){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")
  ```
  brew install carthage
  ```
  {: codeblock}

## Etapa 2. Instalando ferramentas para desenvolvimento local
{: #swift-cli-install-tools}

O {{site.data.keyword.cloud}} fornece ferramentas locais da CLI que ajudam a trabalhar com vários aspectos do {{site.data.keyword.cloud_notm}}. Para obter mais informações, consulte [Informações sobre o {{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli). É possível usar as ferramentas para testar um back-end Swift em uma imagem do Docker local antes da implementação de nuvem.

* Para MacOS e Linux, execute o comando a seguir:
  ```
  curl -sL http://ibm.biz/idt-installer | bash
  ```
  {: codeblock}

* Para Windows 10, execute o comando a seguir como um administrador:
  ```
  Set-ExecutionPolicy Unrestricted; iex(New-Object Net.WebClient).DownloadString('http://ibm.biz/idt-win-installer')
  ```
  {: codeblock}

  Clique com o botão direito no ícone do Windows PowerShell e selecione **Executar como
administrador**.
  {: tip}

## Etapa 3. Criando um aplicativo Swift
{: #create-swift-app-cli}

1. Execute o comando `ibmcloud dev create` por meio da CLI do {{site.data.keyword.dev_cli_short}} para gerar um iniciador pré-configurado. 
  ```
  Ibmcloud dev criar
  ```
  {: codeblock}

  Certifique-se de efetuar login com uma conta do {{site.data.keyword.cloud_notm}} para criar um projeto. Os usuários iniciantes podem [registrar-se ](https://cloud.ibm.com/registration){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") para uma conta gratuita. Use o comando `ibmcloud login` para efetuar login na linha de comandos.
  {: tip}

2. Quando solicitado, selecione as opções 1, depois 6 e, por último, 2, conforme exibido no exemplo a seguir:
  ```
  ? Select a service type:                  
  1. Backend Service / Web App
  2. Mobile Client
  Enter a number> 1

  ? Select a language:
  1. Java - MicroProfile / Java EE
  2. Java - Spring
  3. Node
  4. Python - Django
  5. Python - Flask
  6. Swift
  Enter a number> 6

  ? Select a Starter Kit:
  1. Backend for Frontend - Swift Kitura - A starter for building 
  backend-for-frontend APIs in Swift, using the Kitura framework.
  2. Web App - Create Project
  3. Web App - Swift Kitura Basic - A starter that provides a basic web serving 
  application in Swift, using the Kitura framework.
  Insira um número > 2
  ```
  {: screen}

3. Forneça um nome para o aplicativo:
  ```
  ? Enter a name for your application> swift_project
  ```
  {: screen}

4. Escolha para ativar o suporte ao OpenAPI 2.0:
  ```
  ? Enable your application based on an OpenAPI 2.0 Specification document? [ y/n ] > y
  ```
  {: screen}

  Caso o suporte ao OpenAPI 2.0 esteja ativado, deve-se fornecer um caminho para o documento OpenAPI 2.0 como uma url:
  ```
  ? Path to OpenAPI 2.0 document as a url (both yaml and json formats supported)> http://hostname.domain.com/path/to/file.json

  Gerando aplicativo ...
  ```
  {: screen}

## Etapa 4. Construindo, executando e implementando seu aplicativo
{: #swift-cli-deploy}

Agora é possível construir, executar e implementar seu aplicativo usando o {{site.data.keyword.dev_cli_short}}.

1. **Construção**

  Agora é possível construir seu aplicativo, que é um pré-requisito para executá-lo. Use o comando a seguir na raiz do diretório de aplicativo para construir o seu app:
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Executar
**

  Após uma construção bem-sucedida, é possível executar o aplicativo em um contêiner local com o comando a seguir:
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Um host e uma porta locais para visualizar a página de entrada do seu aplicativo serão exibidos se o comando for executado com êxito.

3. ** Implementar **

  Implemente seu aplicativo no {{site.data.keyword.cloud_notm}} com o comando `deploy`:
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}

## Próximas etapas
{: #swift-cli-next notoc}

Saiba como usar o {{site.data.keyword.cloud_notm}} Developer Console for Apple que permite que os desenvolvedores criem apps por meio de vários kits do iniciador, criem e conectem serviços chave otimizados do {{site.data.keyword.cloud_notm}} e, em seguida, façam rapidamente download do código de trabalho ou configurem para entrega contínua. Os usuários podem criar, visualizar, configurar e gerenciar seu app, bem como fazer download do código do seu aplicativo. Usando o Developer Console for Apple, é possível avaliar e testar rapidamente os serviços do {{site.data.keyword.cloud_notm}} com um app totalmente novo.

Pronto para saltar? Visite o [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") agora para começar.
{: tip}

Para obter mais informações, consulte [Desenvolvendo apps Swift com Starter Kits](/docs/swift/starter_kit?topic=swift-starterkits-intro#starterkits-intro).
