---
title: API メッシュでGraphQL単一ソースメッシュを作成する
description: Adobe Commerce と  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。1 つのソースを持つメッシュの作成について説明します。
landing-page-description: Adobe Commerce と  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。1 つのソースを持つメッシュの作成について説明します。
short-description: Adobe Commerce と  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。1 つのソースを持つメッシュの作成について説明します。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 12%

---

# 単一のソースを持つメッシュを作成する

このビデオでは、Adobe Developer App Builder 用の API メッシュで単一のソースを使用してメッシュを作成する方法を開発者が理解できます。 この基本的な例が期待どおりに動作するには、公にアクセス可能な API またはGraphQLエンドポイントが必要です。 このビデオでは、シンプルな `mesh.json` ファイルを使用して、コマースインスタンスで使用します。 詳細およびコードサンプルについては、 [メッシュを作成する](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## このビデオは誰のためのものですか？

* API メッシュを初めて使用する方
* 複数のGraphQLと API ソースの組み合わせに関心のある開発者
* 「ネットワーク」タブをフィルタリングし、GraphQLでフィルタリングする方法を知っているすべてのユーザー

## ビデオコンテンツ

* API Mesh をリバースプロキシとして使用する
* JSON 設定ファイルからのメッシュの作成
* 新しく作成されたGraphQLエンドポイントへのアクセス

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## json 設定ファイルの作成

API Mesh では、JSON 設定ファイルを使用してソースハンドラーを定義します。 JSON ファイルには、 `sources` メッシュのソースを含む配列。 次に、単一のソースを持つメッシュの例を示します。

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
