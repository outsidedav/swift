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
{:note: .deprecated}

# Mobile Backend as a Service の機能
{: #mbaas}

アプリケーションは、次の 2 つの方法のいずれかを使用して外部機能を使用できます。
* アプリケーションからサービスへの直接的な使用。このようなサービスは通常、Mobile Backend as a Service (MBaaS) プラットフォームの一部としてホストされます。
* ホストされた Backend For Frontend (BFF) コンポーネントを介した使用。このコンポーネントは、サービスの構成と制御を提供し、アプリケーションのバックエンド・ロジックを追加できます。

通常、MBaaS スタイルのアプローチを使用して、モバイル・アプリケーションから外部機能を直接追加する方が簡単です。 最初のサービス・セットを追加する項目とインフラストラクチャーの数が少なくて済みます。ただし、この方法には究極の柔軟性が欠けており、BFF をデプロイすることで可能になるほどの広い範囲で利用することもできません。

{{site.data.keyword.cloud_notm}} プラットフォームの利点の 1 つは、事前に決める必要がないことです。 提供されるサービスはすべて、付属している Swift ベースの SDK を使用してモバイル・デバイスから直接使用できます。 また、{{site.data.keyword.cloud_notm}} プラットフォームは、バックエンド・ロジックを Swift で書くこともサポートしています。これは、{{site.data.keyword.openwhisk_short}} を使用したサーバーレス・モデル、または Kitura などのフレームワークを使用したサーバー・サイド Swift のいずれかを介してサポートされます。 この機能により、BFF モデルを介してバックエンド・ロジックを追加する場合に、Swift を使用して行うことができ、また、Swift SDK の使用法をアプリケーションから BFF に移行することを選択できます。 結果として、MBaaS スタイルのアプローチから BFF のアプローチに、最小限の追加作業で段階的に移行できます。
