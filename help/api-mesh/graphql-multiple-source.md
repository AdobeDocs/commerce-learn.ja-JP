---
title: API メッシュで使用する複数のソースGraphQLを作成する
description: Adobe Commerceで API メッシュに複数のソースを使用する方法と [!DNL Adobe App Builder]. 一般的なエラーとその解決方法について説明します。
landing-page-description: Adobe Commerceおよび [!DNL Adobe App Builder]. 複数のソースを持つリクエストの作成と、一般的なエラーの解決方法について説明します。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# 複数のソースGraphQL API メッシュを作成する

このビデオは、開発者が、複数のソースを使用してGraphQLリバースプロキシを作成する方法を理解するのに役立ちます。 このビデオでは、様々なソースを結び付け、エラーを識別し、Git に変更を保存する方法を示します。 ビデオで使用された基本的なコードサンプルについては、を参照してください。 [メッシュを作成する](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## このビデオは誰のためのものですか？

* API メッシュを初めて使用するユーザー
* 複数の graphql ソースの使用に関心のある開発者
* 「ネットワーク」タブをフィルタリングし、graphql でフィルタリングする方法を知っているユーザー

## ビデオコンテンツ

* 2 つ目のソースの複雑なカスタム属性 API スキーマがデフォルトのソーススキーマを上書きする方法
* 2 番目の上書きスキーマを考慮する API メッシュ設定を変更する
* 名前の競合、スキーマの可用性、その他の SDL 構文など、プロセスで発生する可能性のあるエラーの対処法
* スキーマのステッチを試みた後の一般的なエラーの例
* 編集後に API メッシュを再構築する
* API メッシュ設定を変更した後、Git に変更を保存

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## json 設定ファイルの作成

Adobeの App Builder がすべてのソースを把握できるように、JSON 設定で定義します。 各ソースは配列内の要素で、1 つ以上の要素を持つことができます。 次に、1 つの応答を形成するためにメッシュ化される複数のソースリクエストの例を示します。

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
      },
      {
        "name": "ERP",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
