---

copyright:
  years: 2018
lastupdated: "2018-08-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# スターター・キットを使用した Swift アプリの作成
{: #intro}

{{site.data.keyword.cloud_notm}} Developer Console for Apple は、Apple の開発者がさまざまなスターター・キットからアプリを作成し、{{site.data.keyword.cloud_notm}} 用に最適化された主要サービスをプロビジョンして接続し、作業用コードを素早くダウンロードしたり継続的デリバリーのためのセットアップを行ったりすることを支援します。ユーザーは、アプリのコードをダウンロードできるだけでなく、アプリを作成、表示、構成、管理できます。スターター・キットの使用により、まったく新しいアプリを使用して {{site.data.keyword.cloud_notm}} サービスを素早く評価およびテストすることができます。

開始の準備ができましたか? 今すぐ [{{site.data.keyword.cloud_notm}} Developer Console for Apple](https://console.bluemix.net/developer/appledevelopment/starter-kits) にアクセスして始めましょう。
{: tip}

## スターター・キットについて
{: #starter_kits}

{{site.data.keyword.cloud_notm}} Developer Experience では、さまざまなスターター・キットの中から選択できます。スターター・キットにより、{{site.data.keyword.cloud_notm}} は、開発者が選択した言語で、クラウド・デプロイメントの準備が完了したスケルトン実動アプリを動的にアセンブルします。 各スターター・キットは、コードを再作成するのではなく、コードを再利用できる、特定の実世界のユース・ケースの言語、フレームワーク、およびパターンを具体化します。

スターター・キットは、実動準備が完了しており、(例えば Swift などの) ランタイムを使用してキー・パターンの実装を示すことに重点を置いています。場合によっては、スターター・キットは、サービスの統合を強調するためのシンプルなユーザー・エクスペリエンスを提供します。スターター・キットが、高度なユース・ケースのカスタマイズ可能な実装を表している場合もあります。

スターター・キットには、{{site.data.keyword.cloud_notm}} で、スターター・キットからアプリを作成するときに、移植可能コードを使用して自動的にスキャフォールドされたアプリを作成し、自動的にプロビジョンするリソースを指定できるようにするための手順が含まれています。

## {{site.data.keyword.cloud_notm}} Developer Console for Apple の使用
{: #journey}

{{site.data.keyword.cloud_notm}} Developer Console for Apple には、特定のユース・ケース用の Swift スターター・アプリを作成するためのシームレス・パスが用意されています。その過程で実行するステップを確認しましょう。

### 「概要」画面
{: #overview_screen}

「概要」画面には、Watson、Weather などのユース・ケースのセットに合わせた内容が表示されます。「概要」画面から、ドキュメンテーションの表示、教育リソースのアクセス、サービスの参照、主なスターター・キットの参照や、スターター・キットのより大きなコレクションへのリンクが可能です。左側のナビゲーション領域の`「スターター・キット (Starter Kits)」`をクリックして、「スターター・キット (Starter Kits)」ビューにステップイントゥします。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「概要」画面](images/overview_screen.png "「概要」画面") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple の「概要」画面*

### 「スターター・キット (Starter Kits)」ビュー
{: #starter_kits_view}

「スターター・キット (Starter Kits)」ビューには、ユース・ケース領域に固有のスターター・キットのコレクションが表示されます。スターター・キット・カード上のさまざまなリンクをクリックすると、デモや詳細情報が表示されます。スターター・キットを選択して、「アプリの作成」ビューに移動します。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「スターター・キット (Starter Kits)」ビュー](images/starter_kits_screen.png "「スターター・キット (Starter Kits)」ビュー")<br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple の「スターター・キット (Starter Kits)」ビュー*

### 「アプリの作成」ビュー
{: #create_new_app_view}

**「アプリの作成」**ビューから、アプリに名前を付けたり、デプロイメントおよびルーティング情報を提供したりできます。右側には、アプリの作成時に自動的にプロビジョンするサービス、価格プラン、およびそれぞれのご利用条件も表示されます。`「作成」`をクリックして、「アプリの詳細 (App Details)」ビューに移動します。{{site.data.keyword.cloud_notm}} にログインしていない場合は、ここでログインする必要があります。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリの新規作成 (Create New App)」ビュー](images/create_new_project_screen.png "「アプリの新規作成 (Create New App)」ビュー") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリの新規作成 (Create New App)」ビュー*

## 「アプリの詳細 (App Details)」ビュー
{: #app_details_view}

「アプリの詳細 (App Details)」ビューには、アプリ用に構成されたサービスのリストが表示されます。リスト内の項目ごとに、サービス名、他の情報へのリンク、および 3 つのドットが垂直に並んだ**アクション**・ボタンが表示されます。**アクション**・ボタンのオプションは、アプリからのサービスの削除、サービスのダッシュボードのオープン、サービスの削除用です。サービス・インスタンスを削除すると、このアプリへの関連付けが削除され、サービス・インスタンスは削除されません。また、サービス資格情報はこのビューに統合されるため、個々のサービス・インスタンス・ビューにアクセスして取得する必要はありません。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリの詳細 (App Details)」ビュー](images/project_details_screen.png "「アプリの詳細 (App Details)」ビュー") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリの詳細 (App Details)」ビュー*

「アプリの詳細 (App Details)」ビューを使用することで、新しいサービスまたは既存のサービスを、オリジナルのスターター・キットに含まれていないアプリに追加できます。サービス・リスト・ボックスの**「リソースの追加 (Add Resource)」**リンクをクリックすると、サービスが追加されます。使用可能なサービスは、アプリのタイプと領域で使用可能なサービスによって異なります。したがって、すべてのサービスがすべてのアプリに関連付けられるわけではありません。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「リソースの追加 (Add Resource)」ダイアログ](images/add_resource_screen.png "「リソースの追加 (Add Resource)」ダイアログ") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple の「リソースの追加 (Add Resource)」ダイアログ*

「アプリの詳細 (App Details)」ビューでは、2 つの方法でコードにアクセスできます。
*  **「コードのダウンロード (Download Code)」**を選択し、アプリのコードを生成してダウンロードします。

### 「アプリのリスト (App List)」ビュー
{: #app_list_view}

「アプリのリスト (App List)」ビューから作成したアプリをすべてリストできます。ここから、アプリの名前を変更または削除できます。アプリ名の行をクリックすると、「アプリの詳細 (App Details)」ビューに戻ります。

![{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリのリスト (App List)」ビュー](images/project_list_screen.png "「アプリのリスト (App List)」ビュー") <br> *{{site.data.keyword.cloud_notm}} Developer Console for Apple の「アプリのリスト (App List)」ビュー*

詳しくは、 [{{site.data.keyword.cloud_notm}} Developer Console for Apple Learning Resources](https://console.bluemix.net/developer/appledevelopment/learning-resources) を参照してください。
{: tip}
