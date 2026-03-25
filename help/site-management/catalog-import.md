---
title: Adobe Commerceにネイティブに付属しているカタログ読み込みオプションについて説明します
description: カタログをAdobe Commerce ストアに読み込むためのネイティブオプションの一部について説明します。
kt: 13634
doc-type: tutorial
duration: 211
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---

# カタログを読み込むためのオプション

Adobe Commerceにカタログを読み込むには、いくつかのネイティブな方法があります。 各メソッドには、使用の理由と考慮すべき長所と短所があります。

詳細については、以下のいずれかのオプションから選択してください。

>[!BEGINTABS]

>[!TAB 手動]

## 製品の手動作成 {#manual-import}

カタログが限られており、更新の頻度が低い場合は、手作業で作成するのが最適です。 Commerce管理者の使用方法に関する各製品と一部の限定トレーニングに参加するには、時間が必要です。 手作業によるカタログ管理は、多くの実店舗では適切なオプションではありませんが、状況によっては理にかなっていることもあります。 このプロセスに関する追加のドキュメントについては、[製品の作成](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html){target="_blank"}を参照してください。 複数の方法でカタログを管理することもできますが、オートメーションを使用する場合は、手作業での編集を制限する必要があります。 自動更新では、手動で実行された変更を上書きする機会があるため、混乱が生じます。 カタログを管理するためのAdobe Commerceとの統合がオートメーションとAPIを使用している場合は、管理者から[ ユーザーの役割と権限](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html){target="_blank"}を通じてカタログの管理を制限することをお勧めします。

### このアプローチを検討するタイミング

* 非常に小さいカタログ（例：50個未満）
* アップデートは頻繁にありません
* 製品の詳細、画像、動画がすべて揃っているので、データをCSVに変換する方法を学ぶのに時間がかかりたくありません
* 商品を作成する際に、画像やビデオを追加する
* お客様のチームは`not`様がAPIとOAUTHの仕組みについて詳しく知っています

>[!TAB 管理者CSV]

## 管理者CSV読み込みツール {#admin-csv}

このツールを使用すると、ストア所有者はコマース管理者からCSVを使用してカタログを直接インポートできます。
[Commerce管理者からのデータの読み込み](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}

長所：
管理者からCSVをアップロードすることは、カタログ管理への直接的なアプローチです。 これにより、カタログ製品の更新を中程度のサイズに高速化できます。

短所：

* 遅い
* アップロードファイルの最大サイズはサーバーで定義されており、ストア所有者が簡単に調整できない場合があります。
* 管理者アクセスが必要で、誰かがアクションを実行する必要があるため、自動化は制限されています
* スケジュールの読み込みは、1日の最大1倍に制限されています
* 関連する画像とビデオは個別にアップロードする必要があります

### このアプローチを検討するタイミング

* カタログのサイズが中程度
* 更新は1日1回以下です
* ファイルの最大アップロードサイズを増やす必要がある場合は、サーバー設定にアクセスできます
* お客様のチームは`not`様がAPIとOAUTHの仕組みについて詳しく知っています

>[!TAB Bulk REST API]

## Bulk REST API {#bulk-rest-api}

Bulk REST APIを使用すると、自動化とより頻繁な更新が可能になります。 このAPIは、CSVの管理者アップロードを使用するよりも高速です。
[一括エンドポイントドキュメント ](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

長所：
CSV形式ではない大規模なデータセットをインポートする機能。

短所：

* 関連する画像とビデオは個別にアップロードする必要があります
* ホスティングプロバイダーの帯域幅の制約によって制限される可能性があります

### このアプローチを検討するタイミング

* カタログは任意のサイズ
* アップデートは頻繁に行われ、1日に1倍以上の頻度で行うことができます
* インポートまでの時間は重要ですが、重要ではありません。また、インポートデータの処理における短い遅延も許容されます
* データはCSV形式の構造化されておらず、自動化を使用して変換することはできません

>[!TAB 非同期REST API]

## 非同期REST API {#async-rest-api}

非同期web エンドポイントは、Web APIへのメッセージをインターセプトし、メッセージキューに書き込みます。 システムがそのようなAPI リクエストを受け入れるたびに、UUID ID IDを生成します。 Adobe Commerceは、メッセージをキューに追加するときに、このUUIDを含めます。 次に、消費者はキューからメッセージを読み取り、1つずつ実行します。
[非同期web エンドポイントのドキュメント ](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

長所：

* データのインポートを高速化
* ストア範囲がサポートされているか、`all`を指定して、既存のすべてのストアで操作を実行できます

短所：

* GET リクエストはサポートされていません

### このアプローチを検討するタイミング

* 輸入は頻繁に
* APIを介して送信され、メッセージキューから処理されるまでわずかな遅延はありません。


>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

このAPI オプションを使用すると、他のすべてのネイティブオプションと比較して、非常に高速な読み込みが可能になります。

[ データ REST CSV APIのインポート ](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
長所：

* 受信データを処理する最速の方法
* 1日に複数回実行できます
* HTTP リクエストサイズの制限を回避するために、大規模なリクエストの場合はgzipを使用してデータを圧縮できます。

短所：

* 関連する画像とビデオは個別にアップロードする必要があります
* データはCSV形式である必要があります

### このアプローチを検討するタイミング

* カタログは任意のサイズ
* アップデートは頻繁に行われ、1日に1倍以上の頻度で行うことができます
* インポートにかかる時間を短縮することが重要です
* データはすでにCSV形式であるか、自動化によって容易に変換できます

>[!ENDTABS]

## 関連資料

* [新しいREST CSV](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}を使用してデータを読み込む
* [ データのメインドキュメントの読み込み](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}
* [Adobe Commerce バージョン 2.4.6 リリースノート ](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html){target="_blank"}
* [ ユーザー、役割、権限](../site-management/users-roles-permissions.md){target="_blank"}
