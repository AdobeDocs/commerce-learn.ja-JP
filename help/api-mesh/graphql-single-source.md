---
title: API MeshでのGraphQL シングルソースメッシュの作成
description: Adobe CommerceとAdobe App BuilderでAPI Meshを使用する方法について説明します。 1つのGraphQL ソースでメッシュを作成し、新しいエンドポイントにアクセスする方法について説明します。
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# 1つのソースでメッシュを作成する

このビデオでは、Adobe Developer App Builder用API Meshで、1つのソースを持つメッシュを作成する方法について説明します。 この基本的な例を機能させるには、公開されているAPIまたはGraphQL エンドポイントが必要です。 このビデオでは、Commerce インスタンスで使用するシンプルな`mesh.json` ファイルを作成する方法についても説明します。 詳細とコードサンプルについては、[&#x200B; メッシュの作成](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* API Meshの初心者
* 複数のGraphQLソースとAPI ソースの組み合わせに関心のある開発者
* 「ネットワーク」タブをフィルタリングし、GraphQLでフィルタリングする方法を知る必要があるユーザー

## ビデオコンテンツ

* リバースプロキシとしてのAPI メッシュの使用
* JSON設定ファイルからのメッシュの作成
* 新しく作成したGraphQL エンドポイントへのアクセス

>[!VIDEO](https://video.tv.adobe.com/v/3414124?learn=on)

## JSON設定ファイルの作成

API Meshでは、JSON設定ファイルを使用してソースハンドラーを定義します。 JSON ファイルには、メッシュのソースを含む`sources`配列が含まれています。 次に、単一のソースを持つメッシュの例を示します。

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
