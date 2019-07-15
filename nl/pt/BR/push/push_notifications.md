---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-21"

keywords: push swift, swift notifications, push notifications swift, sending push swift, configure service instance swift, provider credentials swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Enviando  {{site.data.keyword.mobilepushshort}}
{: #push_notifications}

Aprimore seu app Swift usando o serviço {{site.data.keyword.mobilepushshort}} no {{site.data.keyword.cloud}} para enviar notificações ao vivo para dispositivos móveis e aplicativos da web.

 - As notificações podem ser entregues para todos os usuários do aplicativo ou para um conjunto selecionado de usuários ou dispositivos.
 - Suporta notificações interativas e silenciosas.
 - Os clientes podem optar por assinar tags ou tópicos específicos para notificação.
 - Permite que o proprietário do app analise o número de dispositivos registrados para receber notificações e o número de notificações enviadas.

É possível optar por usar o serviço {{site.data.keyword.mobilepushshort}} como uma parte do Modelo MobileFirst Services Starter ou como o {{site.data.keyword.cloud_notm}} [Dedicated Services](/docs/dedicated?topic=dedicated-dedicated#dedicated). Também é possível usar um SDK (kit de desenvolvimento de software) e [APIs de REST ](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/notifications/rest-apis/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "External link icon") para desenvolver adicionalmente seus aplicativos clientes.

![Visão geral de push](images/push_notification_lifecycle.jpg "Visão geral de push")

## Antes de iniciar
{: #prereqs-push}

Primeiro, verifique se os pré-requisitos a seguir estão prontos para execução:

 - iOS 8.0 +
 - Xcode 7.3, 8.0
 - Swift 2.3-4.0
 - CocoaPods ou Carthage

## Etapa 1. Criando uma instância do  {{site.data.keyword.mobilepushshort}}
{: #push_create}

1. No catálogo do {{site.data.keyword.cloud_notm}}, clique em **Remoto** > **{{site.data.keyword.mobilepushshort}}**. A tela de configuração de serviço é aberta.
2. Dê à sua instância de serviço um nome ou use o nome predefinido.
3. Clique em
**Criar**.
4. Na área de janela de navegação, clique em **Conexões** para selecionar um app e ligá-lo a seu serviço. É possível ligar a instância de serviço ao seu app posteriormente se ele não estiver vinculado durante a criação.


## Etapa 2. Obtenha suas credenciais do provedor de notificação
{: #get_creds-push}

Para configurar o serviço de Notificações de push, é necessário obter as credenciais necessárias por meio do Apple Push Notification Service (APNs). Siga as etapas aqui para [obter e configurar as credenciais do APNs ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1){: new_window}.


## Etapa 3. Configurar uma instância de serviço
{: #config-resource-push}

Para usar o serviço {{site.data.keyword.mobilepushshort}} para enviar notificações, faça upload do keystore `.p12` que você criou e que tem a chave privada e os certificados SSL que são necessários para construir e publicar seu aplicativo. A API REST também pode ser
usada para fazer upload de um certificado APNs.

Depois que o arquivo `.cer` estiver em seu acesso à cadeia de chaves, exporte-o para seu computador para criar um certificado `.p12`.

Para obter mais informações sobre o uso de APNs, veja [iOS Developer Library: Local and Push Notification Programming Guide ](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "External link icon").

Para configurar o APNs no console de serviços de Notificação push, conclua as etapas:

1. Selecione  ** Configurar **  no console de serviços do  {{site.data.keyword.mobilepushshort}} .
2. Escolha a opção **Móvel** para atualizar
as informações no formulário **Credenciais push de
APNs**.
3. Escolha uma das seguintes opções:
	- Para **Mobile** opção
		1. Selecione **Ambiente de simulação** (desenvolvimento) ou **Produção** (distribuição) e, em seguida, faça upload do certificado `p.12` que você criou.
		  ![Console do {{site.data.keyword.mobilepushshort}}](images/wizard.jpg "Configuração de dispositivo móvel de notificação de push")

		2. No campo **Senha**, insira a senha que está
associada ao arquivo de certificado `.p12`; em seguida, clique em
**Salvar**.
	- Para opção  ** Web **
		- Na seção Push do Safari, atualize o formulário com as informações necessárias.
		- **Nome do website**: o nome do website fornecido no centro de Notificação.
		- **ID de push do website**: atualize com a sequência de domínio reversa para seu
ID de push do website. Por exemplo, `web.com.example.www`.
		- **URL do website**: forneça a URL do website que está inscrita para enviar notificações de push. Por exemplo, `https://www.example.com`.
		- **Domínios permitidos**: (parâmetro opcional) uma lista de websites que solicitam permissão do usuário. Certifique-se de que as URLs sejam valores separados por vírgula. Os valores na URL do website serão usados se as informações não forem fornecidas.
		- **Sequência de formatações de URL**: a URL a ser resolvida quando a notificação é clicada. Por exemplo, `https://www.example.com`. Assegure-se de que a URL use o esquema http ou https.
		-**Certificado push da web do Safari**: faça upload do certificado `.p12` e forneça a senha.
4. Clique em  ** Salvar **.

  ![Console do {{site.data.keyword.mobilepushshort}}](images/push_configure_safari.jpg "Configuração da web de notificação de push")

## Etapa 4. Configurar SDK do cliente de serviço
{: #service-client-push}

Para ativar os aplicativos iOS para receber notificações push para seus dispositivos, é necessário configurar o iOS SDK do serviço {{site.data.keyword.mobilepushshort}}.

Os SDKs do {{site.data.keyword.cloud_notm}} Mobile Services Swift podem ser instalados com o Cocoapods ou o Carthage. Para obter mais informações, consulte [https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Etapa 5. Enviando uma Notificação
{: #send-notify-push}

Depois de desenvolver seu aplicativo, é possível enviar notificações push básicas.

Para enviar notificações push básicas, conclua as etapas a seguir:

1. Selecione **Enviar notificações** e componha uma mensagem
escolhendo uma opção **Enviar para**. As opções suportadas são **Dispositivo por tag**, **ID do dispositivo**, **ID do usuário**, **Dispositivos iOS**, **Notificações da web** e **Todos os dispositivos**.
**Nota**: quando você seleciona a opção **Todos os dispositivos**, todos os dispositivos que estão inscritos para {{site.data.keyword.mobilepushshort}} recebem notificações.

  ![Tela Enviar notificações](images/tag_notification.jpg "Tela Enviar notificações")

2. No campo **Mensagem**, componha sua mensagem. Escolha a
configurar das definições opcionais conforme necessário.
3. Clique em  ** Enviar **.
3. Verifique se os dispositivos ou o navegador receberam a notificação.

  A captura de tela a seguir mostra uma caixa de alerta que manipula uma notificação de push em primeiro plano no dispositivo.
  
  ![Notificação de push de primeiro plano no Android](images/Android_Screenshot.jpg "Alerta de notificação de primeiro plano")

  A seguinte captura de tela mostra uma notificação de push em segundo plano.
  
  ![Notificação de push de segundo plano no iOSd](images/background.png "Alerta de notificação de segundo plano")

### Configurações opcionais
{: #optional-push}

É possível customizar as configurações do {{site.data.keyword.mobilepushshort}} para enviar notificações para dispositivos iOS. As opções de customização opcionais a seguir são suportadas.

- **Badge**: indica o número que é exibido no badge do aplicativo. O valor padrão é zero (0) e não exibe um badge.
- **Som**: indica que um clique de som seja reproduzido no recebimento de uma notificação. Suporta o padrão ou o nome de um recurso válido empacotado no app.
- **Carga útil adicional**: especifica os valores de carga útil customizados para suas notificações.

Também é possível escolher ativar [notificações interativas](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") e [notificações de rich media](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Etapa 6. Monitoramento para notificações entregues
{: #monitor-push}

O serviço {{site.data.keyword.mobilepushshort}} fornece um utilitário de monitoramento para ajudá-lo a verificar o status das mensagens que são enviadas. Para configurar o seu utilitário de monitoramento, consulte [Ativar monitoramento para aplicativos do iOS](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo").

## Próximas etapas
{: #next-push notoc}

 - Para saber mais sobre o serviço e aproveitar todos os recursos, leia toda a nossa [documentação](/docs/services/mobilepush?topic=mobile-pushnotification-overview-push).

 - Para obter uma introdução ao trabalho com serviços Mobile e o {{site.data.keyword.cloud_notm}}, consulte [Introdução aos apps Mobile no {{site.data.keyword.cloud_notm}}](/docs/services/mobile?topic=mobile-about).

 - Os kits do iniciador são uma das maneiras mais rápidas de usar os recursos do {{site.data.keyword.cloud_notm}}. Visualize os nossos kits do iniciador disponíveis no [Painel do Mobile Developer](https://{DomainName}/developer/mobile/dashboard){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"). Faça download do código. Execute o app.

 - É possível usar a [IU do Swagger](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/notifications/rest-apis/){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") para revisar rapidamente a documentação da API de REST.
