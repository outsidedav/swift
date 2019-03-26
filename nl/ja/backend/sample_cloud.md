---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# iOS と Cloud のロジックのユース・ケース
{: #sample_cloud}

Backend For Frontend (BFF) を使用する iOS アプリケーションの例については、[画像共有サンプル・アプリケーションの BluePic](https://github.com/IBM/BluePic) を参照してください。 BluePic アプリは以下のテクノロジーを使用します。

* Object Storage と Cloudant を使用して画像データを保管します。
* Watson Visual Recognition と IBM Weather Company サービスを使用して、追加情報を画像に追加します。
* Kitura と {{site.data.keyword.openwhisk}} を使用して、バックエンドのロジックを提供し、認証とプッシュ通知を制御します。
これらはすべて Swift で記述されます。

![BluePic](images/cloudlogic.png "BluePic のフロー")

BluePic の例では、画像が投稿されると、Cloudant にそのデータが記録され、Object Storage に画像のバイナリーが保管されます。 その場所から {{site.data.keyword.openwhisk_short}} シーケンスが呼び出され、画像のアップロード元の場所に基づいて、温度や現在の状態 (例えば晴れや曇り) などの気象データが計算されます。 {{site.data.keyword.openwhisk_short}} シーケンス内で Watson Visual Recognition を使用して、画像を分析したり、画像の内容に基づいてテキスト・タグを抽出したりすることもできます。 最後に、プッシュ通知がユーザーに送信され、画像が処理されたことが通知され、この時点で気象データとタグのデータが含まれています。
