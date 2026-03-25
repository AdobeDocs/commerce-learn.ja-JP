---
title: API Meshで使用する複数のソース GraphQLの作成
description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshに複数のソースを使用する方法について説明します。 よくあるエラーとその解決方法について説明します。
landing-page-description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshを使用する方法について説明します。 複数のソースを持つメッシュを作成する方法と、一般的なエラーを解決する方法について説明します。
short-description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshを使用する方法について説明します。 複数のソースを持つメッシュを作成する方法と、一般的なエラーを解決する方法について説明します。
kt: 11804
doc-type: tutorial
duration: 409
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# 複数のソースを持つメッシュを作成する

このビデオでは、Adobe Developer App Builder用API Meshで複数のソースを持つメッシュを作成する方法について説明します。 このビデオでは、複数のソースを持つメッシュを作成し、エラーを特定する方法を説明します。 詳細とコードサンプルについては、[ メッシュの作成](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* API Meshを初めて利用する方
* 複数のAPI ソースとGraphQLソースの組み合わせに関心のある開発者

## ビデオコンテンツ

* [transforms](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"}を使用してデフォルトのソーススキーマを変更する方法
* 名前の競合、スキーマの可用性、その他のスキーマ構文の問題などのエラーのトラブルシューティング方法
* 変更された設定でメッシュを更新する

>[!VIDEO](https://video.tv.adobe.com/v/3414125?learn=on)

## json設定ファイルの作成

API Meshでは、JSON設定ファイルを使用してソースハンドラーを定義します。 JSON ファイルには、メッシュのソースを含む`sources`配列が含まれています。 複数のソースを持つメッシュの例を次に示します。

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
        "name": "Example",
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
