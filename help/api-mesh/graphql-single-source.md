---
title: API MeshでのGraphQL シングルソースメッシュの作成
description: Adobe CommerceとAdobe App BuilderでAPI Meshを使用する方法について説明します。 1つのGraphQL ソースでメッシュを作成し、新しいエンドポイントにアクセスする方法について説明します。
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
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

>[!VIDEO](https://video.tv.adobe.com/v/3419723?captions=jpn&learn=on)

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
