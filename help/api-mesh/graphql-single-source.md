---
title: API メッシュでGraphQL単一ソースメッシュを作成する
description: Adobe Commerceおよび [!DNL Adobe App Builder]. 1 つのソースを持つメッシュの作成について説明します。
landing-page-description: Adobe Commerceおよび [!DNL Adobe App Builder]. 1 つのソースを持つメッシュの作成について説明します。
short-description: Discover how to use API Mesh on Adobe Commerce and [!DNL Adobe App Builder]. Learn about creating a mesh that has one source.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 単一のソースでメッシュを作成する

このビデオでは、Adobe Developer App Builder 用の API メッシュで単一のソースを使用してメッシュを作成する方法を開発者が理解できます。 この基本的な例が期待どおりに動作するには、公にアクセス可能な API またはGraphQLエンドポイントが必要です。 このビデオでは、シンプルな `mesh.json` ファイルを使用して、コマースインスタンスで使用します。 詳細およびコードサンプルについては、 [メッシュを作成する](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## このビデオは誰のためのものですか？

* API メッシュを初めて使用する方
* 複数のGraphQLと API ソースの組み合わせに関心のある開発者
* 「ネットワーク」タブをフィルタリングし、GraphQLでフィルタリングする方法を知っているすべてのユーザー

## ビデオコンテンツ

* API Mesh をリバースプロキシとして使用
* JSON 設定ファイルからのメッシュの作成
* 新しく作成されたGraphQLエンドポイントへのアクセス

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## json 設定ファイルの作成

API Mesh では、JSON 設定ファイルを使用してソースハンドラーを定義します。 JSON ファイルには `sources` メッシュのソースを含む配列。 単一のソースを持つメッシュの例を次に示します。

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
