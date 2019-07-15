---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: bluepic swift, ios bluepic, bff swift image, sample swift, swift example bff

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Caso de Uso de Lógica do iOS e da Nuvem
{: #sample_cloud}

Um exemplo de um aplicativo iOS que usa um Backend For Frontend (BFF) pode ser visto por meio do [aplicativo de amostra de compartilhamento de imagem do BluePic](https://github.com/IBM/BluePic){: new_window} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo"). O app BluePic usa as tecnologias a seguir:

* Object Storage e Cloudant para armazenar os dados de imagem.
* O Watson Visual Recognition e o serviço IBM Weather Company para incluir informações adicionais nas imagens.
* Kitura e {{site.data.keyword.openwhisk}} para fornecer lógica de backend, autenticação de controle e notificações push, todos gravados em Swift.

![BluePic](images/cloudlogic.png "BluePic Flow")

No exemplo do BluePic, quando uma imagem é postada, os seus dados são registrados no Cloudant, e o binário da imagem é armazenado no Object Storage. A partir daí, uma sequência do {{site.data.keyword.openwhisk_short}} é chamada e faz com que dados de clima, como temperatura e condição atual (por exemplo, ensolarado, nublado) sejam calculadas com base no local do qual uma imagem foi transferida por upload. O Watson Visual Recognition também é usado na sequência do {{site.data.keyword.openwhisk_short}} para analisar a imagem e extrair tags de texto com base no conteúdo da imagem. Finalmente, uma notificação push é enviada para o usuário, informando-o de que a imagem foi processada e agora inclui dados de clima e de tag.
