---
title: Adobe Commerceにネイティブなカタログ読み込みオプションについて説明します
description: カタログをAdobe Commerceストアに読み込むためのネイティブオプションのいくつかを説明します。
kt: 13634
doc-type: tutorial
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
source-git-commit: 273119420a7051b1833d9b796bdce36e17d893c7
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 0%

---

# カタログを読み込むためのオプションについて説明します。

カタログをAdobe Commerceに読み込むには、いくつかのネイティブな方法があります。 それぞれの方法には、使い方に関する独自の推論と、考慮する必要のある長所と短所があります。

詳しくは、以下のオプションのいずれかを選択してください。

>[!BEGINTABS]

>[!TAB 手動]

## 製品の手動作成 {#manual-import}

限られたカタログがあり、更新が頻繁でない場合は、手動で作成するのが最適な方法です。 各製品と、コマース管理者の使用方法に関する一部の限定的なトレーニングを入力する時間が必要です。 ほとんどの店舗では、手動のカタログ管理は適切なオプションではありませんが、状況によっては合理的な場合もあります。 このプロセスに関する追加のドキュメントを参照するには、 [製品の作成](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html){target="_blank"}. Do not forget, you can use more than one method to manage your catalog, however once automation is used, manual edits must be limited. Automated updates have the opportunity to overwrite any changes performed manually, and therefore cause confusion. Once the integration with Adobe Commerce to manage the catalog is using automation and APIs, it is advised to restrict management of the catalog from the admin through [user roles and permissions](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html){target="_blank"}.



### このアプローチを検討するタイミング

- 非常に小さいカタログ（例：50 個未満の製品）
- 更新はまれです
- すべての製品の詳細、画像、ビデオが揃っているので、データを CSV に変換する方法を学ぶのに時間をかけたくはありません
- 製品の作成時に画像やビデオの追加を含める
- チームが `not` API と OAUTH の仕組みについて



>[!TAB 管理 CSV]

## CSV 読み込みツールの管理 {#admin-csv}

このツールを使用すると、ストアの所有者は、コマース管理者から CSV を使用してカタログを読み込むことができます。
[コマース管理からデータをインポート](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}

長所：管理者から CSV をアップロードする方法は、カタログ管理に対する簡単な方法です。 適度なサイズのカタログに対して、カタログ製品の更新を高速化できます。

短所：

- 遅い
- サーバー上で定義される最大アップロードファイルサイズ。ストアの所有者が容易に調整できない場合があります。
- 管理者アクセス権とアクションを実行するユーザーが必要です。自動処理は制限されます
- スケジュールのインポートは 1 日に 1 回までに制限されています
- 関連する画像とビデオは別々にアップロードする必要があります



### このアプローチを検討するタイミング

- カタログのサイズは適度です
- 更新は 1 日に 1 回以下です
- 最大ファイルアップロードサイズを増やす必要がある場合に備えて、サーバー設定に対するアクセス権がある
- チームが `not` API と OAUTH の仕組みについて



>[!TAB 一括 REST API]

## 一括 REST API {#bulk-rest-api}

一括 REST API を使用すると、自動化や頻繁な更新が可能です。 この API は、CSV の管理者アップロードを使用するよりも高速です。
[一括エンドポイントドキュメント](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

長所： CSV 形式でない大きなデータセットを読み込む機能。

短所：

- 関連する画像とビデオは別々にアップロードする必要があります
- ホスティングプロバイダーの帯域幅の制約によって制限可能
- ラベルではなくオプション属性 ID を使用する必要があります



### このアプローチを検討するタイミング

- カタログは任意のサイズです
- 更新は頻繁に行われ、1 日に 1 回より多くの場合は有効です
- インポートする時間は重要ですが、重要ではありません
- データは CSV 形式で構造化されておらず、自動化を使用して変換できません



>[!TAB 非同期 REST API]

## 非同期 REST API {#async-rest-api}

非同期 Web エンドポイントは、Web API へのメッセージを傍受し、メッセージキューに書き込みます。 システムは、このような API リクエストを受け入れるたびに、UUID 識別子を生成します。 Adobe Commerceは、メッセージをキューに追加する際に、この UUID を含めます。 次に、コンシューマーがキューからメッセージを読み取り、1 つずつ実行します。
[非同期 Web エンドポイントドキュメント](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

長所：

- 高速でデータをインポート
- ストアの範囲がサポートされているか、指定できます `all` 既存のすべての店舗で操作を実行するには

短所：

- GETリクエストはサポートされていません
- ラベルの代わりにオプション属性 ID を使用する必要があります


### このアプローチを検討するタイミング

- インポートは頻繁です
- API を介して送信されてから、メッセージキューから処理されるまでの時間に多少の遅延が生じても問題はありません。



>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

この API オプションを使用すると、他のすべてのネイティブオプションと比べて非常に高速なインポートが可能です。

[データ REST CSV API の読み込み](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
長所：

- 受信データを処理する最速の方法
- 1 日に複数回実行可能
- 大きなリクエストの場合は、HTTP リクエストのサイズ制限を避けるために、gzip を使用してデータを圧縮できます。

短所：

- 関連する画像とビデオは別々にアップロードする必要があります
- ラベルではなくオプション属性 ID を使用する必要があります
- データは CSV 形式にする必要があります

### このアプローチを検討するタイミング

- カタログは任意のサイズです
- 更新は頻繁に行われ、1 日に 1 回より多くの場合は有効です
- インポートに要する全体的な時間は重要です
- データは既に CSV 形式になっているか、自動化を使用して簡単に変換できます



>[!ENDTABS]

## その他のリソース

- [新しい REST CSV を使用したデータのインポート](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [データのメインドキュメントのインポート](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}
- [Adobe Commerceバージョン 2.4.6 リリースノート](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html){target="_blank"}
- [ユーザー、役割、権限](../site-management/users-roles-permissions.md){target="_blank"}
