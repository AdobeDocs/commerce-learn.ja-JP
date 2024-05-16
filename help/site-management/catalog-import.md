---
title: Adobe Commerceに付属しているカタログインポートオプションについて説明します
description: カタログをAdobe Commerce ストアに読み込むためのネイティブオプションをいくつか説明します。
kt: 13634
doc-type: tutorial
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
source-git-commit: 47a71d3523d5a894ca4edc458f7e2cf71c283618
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 0%

---

# カタログの読み込みオプション

カタログをAdobe Commerceに読み込むためのネイティブな方法がいくつかあります。 各方法には、使用する理由と、考慮する必要があるメリットとデメリットがあります。

詳しくは、以下のいずれかのオプションを選択してください。

>[!BEGINTABS]

>[!TAB 手動]

## 製品の手動作成 {#manual-import}

カタログの数が限られ、更新の頻度が低い場合は、手動で作成するのが最適な方法です。 各製品を表示するには時間がかかり、Commerce Admin の使用方法に関するいくつかの限定的なトレーニングが必要です。 手動のカタログ管理は、ほとんどの店舗にとって適切なオプションではありませんが、特定の状況では意味がある場合があります。 このプロセスに関する追加のドキュメントを参照するには、 [製品の作成](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html){target="_blank"}. カタログの管理には複数の方法を使用できますが、自動化を使用すると、手動の編集を制限する必要があります。 自動更新では、手動で実行された変更を上書きできるので、混乱が生じます。 カタログを管理するためにAdobe Commerceと統合して自動処理と API を使用している場合は、管理者が以下を使用してカタログの管理を制限することをお勧めします [ユーザーの役割と権限](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html){target="_blank"}.



### このアプローチを検討すべきタイミング

- 非常に小さいカタログ（例：製品 50 個未満）
- アップデートの頻度は低い
- すべての製品詳細、画像、ビデオがあり、データを CSV に変換する方法を時間をかけて学習する必要はありません
- 製品の作成時に画像やビデオの追加を含める
- あなたのチームはです `not` api と OAUTH の仕組みに精通している



>[!TAB 管理 CSV]

## 管理 CSV 読み込みツール {#admin-csv}

このツールを使用すると、ストア所有者は、コマース管理から CSV を使用してカタログを読み込むことができます。
[Commerce管理者からのデータの読み込み](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}

長所：管理者から CSV をアップロードするのは、カタログ管理への簡単なアプローチです。 これにより、適度なサイズのカタログに対して、カタログ製品のアップデートを迅速に実行できます。

短所：

- 遅い
- 最大アップロードファイルサイズはサーバーで定義されており、ストア所有者が簡単に調整できない場合があります。
- 管理者アクセス権と、アクションを実行するユーザーが必要です。自動化は制限されています
- スケジュールの読み込みは 1 日の最大 1 倍に制限されています
- 関連付けられた画像とビデオは個別にアップロードする必要があります



### このアプローチを検討すべきタイミング

- カタログのサイズは中程度です
- 更新は 1 日に 1 回までです
- 最大ファイルアップロードサイズを増やす必要が生じた場合に備えて、サーバー設定にいくらかアクセスできます
- あなたのチームはです `not` api と OAUTH の仕組みに精通している



>[!TAB 一括 REST API]

## 一括 REST API {#bulk-rest-api}

一括 REST API を使用すると、自動化とより頻繁な更新が可能になります。 この API は、CSV の管理者アップロードを使用する場合よりも高速です。
[一括エンドポイントのドキュメント](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

長所：CSV 形式ではない大きなデータセットを読み込む機能。

短所：

- 関連付けられた画像とビデオは個別にアップロードする必要があります
- ホスティングプロバイダーの帯域幅の制約によって制限される可能性がある
- ラベルではなく、オプション属性 ID を使用する必要があります



### このアプローチを検討すべきタイミング

- カタログのサイズは任意です
- 頻繁に更新されますが、1 日に 1 回を超えてもかまいません
- 読み込みにかかる時間は重要ですが、重要ではありません。また、読み込みデータの処理に短時間の遅延が生じた場合でも問題ありません
- データは CSV 形式で構造化されておらず、自動化を使用して変換することはできません



>[!TAB 非同期 REST API]

## 非同期 REST API {#async-rest-api}

非同期 web エンドポイントは、Web API へのメッセージをインターセプトして、メッセージキューに書き込みます。 システムはこのような API リクエストを受け入れるたびに、UUID 識別子を生成します。 Adobe Commerceは、メッセージをキューに追加する際に、この UUID を含めます。 次に、コンシューマーはキューからメッセージを読み取り、1 つずつ実行します。
[非同期 web エンドポイントのドキュメント](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

長所：

- データを迅速にインポート
- ストアの範囲はサポートされています。または、以下を指定できます `all` 既存のすべてのストアに対して操作を実行するには

短所：

- GETリクエストはサポートされていません

### このアプローチを検討すべきタイミング

- 輸入が多い
- API 経由で送信され、メッセージキューから処理されるまでの少しの遅延は問題になりません。



>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

この API オプションを使用すると、他のすべてのネイティブオプションと比較して、非常に高速に読み込むことができます。

[データ REST CSV API の読み込み](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
長所：

- 受信データを処理する最速の方法
- 1 日に複数回実行できます
- 大きなリクエストに対しては、HTTP リクエストのサイズ制限を回避するために、gzip を使用してデータを圧縮できます。

短所：

- 関連付けられた画像とビデオは個別にアップロードする必要があります
- データは CSV 形式である必要があります

### このアプローチを検討すべきタイミング

- カタログのサイズは任意です
- 頻繁に更新されますが、1 日に 1 回を超えてもかまいません
- 読み込みにかかる全体的な時間は重要です
- データは既に CSV 形式になっています。または、自動処理を使用して簡単に変換できます



>[!ENDTABS]

## その他のリソース

- [新しい REST CSV を使用したデータの読み込み](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [データの読み込みのメインドキュメント](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}
- [Adobe Commerce バージョン 2.4.6 リリースノート](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html){target="_blank"}
- [ユーザー、役割、権限](../site-management/users-roles-permissions.md){target="_blank"}
