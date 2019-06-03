---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: swift starter kit, apple developer console, download code swift, app details swift, create swift app

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# スターター・キットを使用した Swift アプリの作成
{: #starterkits-intro}

{{site.data.keyword.cloud_notm}} Developer Console for Apple は、Apple の開発者がさまざまなスターター・キットからアプリを作成し、{{site.data.keyword.cloud_notm}} 用に最適化された主要サービスをプロビジョンして接続し、作業用コードを素早くダウンロードしたり継続的デリバリーのためのセットアップを行ったりすることを支援します。 ユーザーは、アプリのコードをダウンロードできるだけでなく、アプリを作成、表示、構成、管理できます。 スターター・キットの使用により、まったく新しいアプリを使用して {{site.data.keyword.cloud_notm}} サービスを素早く評価およびテストすることができます。

開始の準備ができましたか? 今すぐ [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://cloud.ibm.com/developer/appledevelopment/starter-kits){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") にアクセスして始めましょう。
{: tip}

## スターター・キットとは
{: #starterkits-what}

{{site.data.keyword.cloud_notm}} Developer Experience では、さまざまなスターター・キットの中から選択できます。 スターター・キットにより、{{site.data.keyword.cloud_notm}} は、開発者が選択した言語で、クラウド・デプロイメントの準備が完了したスケルトン実動アプリを動的にアセンブルします。 各スターター・キットは、コードを再作成するのではなく、コードを再利用できる、特定の実世界のユース・ケースの言語、フレームワーク、およびパターンを具体化します。

スターター・キットは、実動準備が完了しており、(例えば Swift などの) ランタイムを使用してキー・パターンの実装を示すことに重点を置いています。 場合によっては、スターター・キットは、サービスの統合を強調するためのシンプルなユーザー・エクスペリエンスを提供します。 スターター・キットが、高度なユース・ケースのカスタマイズ可能な実装を表している場合もあります。

スターター・キットには、{{site.data.keyword.cloud_notm}} で、スターター・キットからアプリを作成するときに、移植可能コードを使用して自動的にスキャフォールドされたアプリを作成し、自動的にプロビジョンするサービスを指定できるようにするための手順が含まれています。

## {{site.data.keyword.cloud_notm}} Developer Console for Apple の使用
{: #starterkits-journey}

{{site.data.keyword.cloud_notm}} Developer Console for Apple には、特定のユース・ケース用の Swift スターター・アプリを作成するためのシームレス・パスが用意されています。 その過程で実行するステップを確認しましょう。

### 「概要」画面
{: #overview_screen}

「概要」画面には、Watson、Weather などのユース・ケースのセットに合わせた内容が表示されます。 「概要」画面から、ドキュメンテーションの表示、教育リソースのアクセス、サービスの参照、主なスターター・キットの参照や、スターター・キットのより大きなコレクションへのリンクが可能です。 ナビゲーション領域の**「スターター・キット」**を選択して、「スターター・キット」ビューにステップイントゥします。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「概要」画面](images/overview_screen.png "「概要」画面")

### 「スターター・キット (Starter kits)」ビュー
{: #starter_kits_view}

「スターター・キット (starter kits)」ビューには、ユース・ケース領域に固有のスターター・キットのコレクションが表示されます。 スターター・キット・カード上のさまざまなリンクをクリックすると、デモや詳細情報が表示されます。 スターター・キットを選択して、「アプリの作成」ビューに移動します。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「スターター・キット」ビュー](images/starter_kits_screen.png "「スターター・キット」ビュー")

### 「アプリの作成」ビュー
{: #create_new_app_view}

**「アプリの作成」**ビューから、アプリに名前を付けたり、デプロイメントおよびルーティング情報を提供したりできます。 アプリの作成時に自動的にプロビジョンするサービス、価格プラン、およびそれぞれのご利用条件も表示されます。**「作成」**を選択して、「アプリの詳細 (App Details)」ビューに移動します。{{site.data.keyword.cloud_notm}} にログインしていない場合は、ここでログインする必要があります。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリの新規作成 (Create New App)」ビュー](images/create_new_project_screen.png "「アプリの新規作成 (Create New App)」ビュー")

## 「アプリの詳細 (App details)」ビュー
{: #app_details_view}

「アプリの詳細 (App Details)」ビューには、アプリ用に構成されたサービスのリストが表示されます。 リスト内の項目ごとに、サービス名、他の情報へのリンク、および 3 つのドットが垂直に並んだ**アクション**・ボタンが表示されます。 **アクション**・ボタンのオプションは、アプリからのサービスの削除、サービスのダッシュボードのオープン、サービスの削除用です。 サービス・インスタンスを削除すると、このアプリへの関連付けが削除され、サービス・インスタンスは削除されません。 また、サービス資格情報はこのビューに統合されるため、個々のサービス・インスタンス・ビューにアクセスして取得する必要はありません。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリの詳細 (App Details)」ビュー](images/project_details_screen.png "「アプリの詳細 (App Details)」ビュー")

**「アプリの詳細 (App details)」**ページを使用することで、元のスターター・キットに含まれていない新しいサービスまたは既存のサービスをアプリに追加できます。 **「サービスの追加」**をクリックして、サービスを追加します。 使用可能なサービスは、アプリのタイプと領域で使用可能なサービスによって異なります。したがって、すべてのサービスがすべてのアプリに関連付けられるわけではありません。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「リソースの追加」ダイアログ](images/add_resource_screen.png "「リソースの追加」ダイアログ")

### コードのダウンロード

_「アプリの詳細 (App details)」_ページでは、**「コードのダウンロード (Download code)」**を選択してコードにアクセスして、アプリのコードを生成したりダウンロードしたりすることができます。

### 「アプリのリスト (App List)」ビュー
{: #app_list_view}

_「アプリのリスト (App List)」_ビューから作成したアプリをすべてリストできます。 ここから、アプリの名前を変更または削除できます。 アプリ名の行をクリックすると、「アプリの詳細 (App Details)」ビューに戻ります。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリのリスト (App List)」ビュー](images/project_list_screen.png "「アプリのリスト (App List)」ビュー")

詳しくは、[{{site.data.keyword.cloud_notm}} Developer Console for Apple Learning Resources](https://cloud.ibm.com/developer/appledevelopment/learning-resources){: new_window} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")を参照してください。
{: tip}
