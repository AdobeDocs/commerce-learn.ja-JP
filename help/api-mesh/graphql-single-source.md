---
title: API メッシュで使用するGraphQL単一ソースリクエストを作成します
description: Adobe Commerceおよび [!DNL Adobe App Builder]. 1 つのソースを持つリクエストの作成について説明します。
landing-page-description: Adobe Commerceおよび [!DNL Adobe App Builder]. 1 つのソースを持つリクエストの作成について説明します。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 単一ソースのGraphQL API メッシュを作成する

このビデオは、開発者がGraphQLリバースプロキシの作成方法を理解するのに役立ち、1 つのソースを持つ場合に役立ちます。 GraphQL Mesh が期待どおりに動作するには、有効なGraphQLスキーマを持つ公開アクセス可能な URL が必要です。 また、コマース Web サイトで使用する最初の JSON を設定する方法についても説明します。 ビデオで使用された基本的なコードサンプルについては、を参照してください。 [メッシュを作成する](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## このビデオは誰のためのものですか？

* API メッシュを初めて使用するユーザー
* 複数の graphql ソースの使用に関心のある開発者
* 「ネットワーク」タブをフィルタリングし、graphql でフィルタリングする方法を知っているユーザー

## ビデオコンテンツ

* API をリバースプロキシとして使用する
* Adobe Developerコマンドラインインターフェイスを使用した JSON 設定
* 新しく作成されたGraphQLエンドポイントへのアクセス

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## json 設定ファイルの作成

Adobeの App Builder がすべてのソースを把握できるように、JSON 設定で定義します。 各ソースは配列内の要素で、1 つ以上の要素を持つことができます。 次に、単一のソースの例を示します

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
