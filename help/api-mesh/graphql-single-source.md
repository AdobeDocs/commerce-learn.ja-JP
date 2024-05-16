---
title: API メッシュでのGraphQL シングルソースメッシュの作成
description: Adobe Commerceで API メッシュを使用する方法と [!DNL Adobe App Builder]. 1 つのソースを持つメッシュの作成について説明します。
landing-page-description: Adobe Commerceで API メッシュを使用する方法と [!DNL Adobe App Builder]. 1 つのソースを持つメッシュの作成について説明します。
short-description: Adobe Commerceで API メッシュを使用する方法と [!DNL Adobe App Builder]. 1 つのソースを持つメッシュの作成について説明します。
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
source-wordcount: '237'
ht-degree: 0%

---

# 単一のソースを持つメッシュを作成する

このビデオは、Adobe Developer App Builder の API メッシュで 1 つのソースを使用してメッシュを作成する方法を開発者が理解するのに役立ちます。 この基本的な例が期待どおりに動作するには、公開アクセス可能な API またはGraphQL エンドポイントが必要です。 このビデオでは、シンプルなを作成する方法についても説明します `mesh.json` Commerce インスタンスで使用するファイル。 詳細とコードサンプルについては、次を参照してください。 [メッシュを作成する](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## このビデオの目的は誰ですか。

* API メッシュを初めて使用するユーザー
* 複数のGraphQLと API ソースの組み合わせに関心のある開発者
* 「ネットワーク」タブのフィルタリング方法とGraphQLによるフィルタリング方法を知る必要のあるユーザー

## ビデオコンテンツ

* リバースプロキシとしての API メッシュの使用
* JSON 設定ファイルからのメッシュの作成
* 新しく作成されたGraphQL エンドポイントへのアクセス

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## Json 設定ファイルを作成します。

API メッシュでは、JSON 設定ファイルを使用してソースハンドラーを定義します。 JSON ファイルには、が含まれています `sources` メッシュのソースを含む配列。 単一ソースを持つメッシュの例を次に示します。

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
