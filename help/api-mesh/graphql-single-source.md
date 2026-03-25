---
title: API MeshでのGraphQL シングルソースメッシュの作成
description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshを使用する方法について説明します。 1つのソースを持つメッシュの作成について説明します。
landing-page-description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshを使用する方法について説明します。 1つのソースを持つメッシュの作成について説明します。
short-description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshを使用する方法について説明します。 1つのソースを持つメッシュの作成について説明します。
kt: 11804
doc-type: tutorial
duration: 510
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# 1つのソースでメッシュを作成する

このビデオでは、Adobe Developer App Builder用API Meshで、1つのソースを持つメッシュを作成する方法について説明します。 この基本的な例を想定どおりに動作させるには、公開アクセス可能なAPIまたはGraphQL エンドポイントが必要です。 このビデオでは、Commerce インスタンスで使用するシンプルな`mesh.json` ファイルを作成する方法についても説明します。 詳細とコードサンプルについては、[&#x200B; メッシュの作成](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* API Meshの初心者
* 複数のGraphQLソースとAPI ソースの組み合わせに関心のある開発者
* 「ネットワーク」タブをフィルタリングし、GraphQLでフィルタリングする方法を知る必要があるユーザー

## ビデオコンテンツ

* リバースプロキシとしてのAPI メッシュの使用
* JSON設定ファイルからのメッシュの作成
* 新しく作成したGraphQL エンドポイントへのアクセス

>[!VIDEO](https://video.tv.adobe.com/v/3414124?learn=on)

## json設定ファイルの作成

API Meshでは、JSON設定ファイルを使用してソースハンドラーを定義します。 JSON ファイルには、メッシュのソースを含む`sources`配列が含まれています。 1つのソースを持つメッシュの例を次に示します。

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
