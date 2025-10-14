---
title: API メッシュで使用する複数のソース GraphQLを作成する
description: Adobe Commerceと  [!DNL Adobe App Builder] で API メッシュに複数のソースを使用する方法を参照してください。 一般的なエラーとその解決方法について説明します。
landing-page-description: Adobe Commerceと  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。 複数のソースを持つメッシュを作成する方法と、一般的なエラーを解決する方法について説明します。
short-description: Adobe Commerceと  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。 複数のソースを持つメッシュを作成する方法と、一般的なエラーを解決する方法について説明します。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# 複数のソースを持つメッシュを作成する

このビデオは、Adobe Developer App Builderの API メッシュで複数のソースを使用してメッシュを作成する方法を開発者が理解するのに役立ちます。 このビデオでは、複数のソースを持つメッシュを作成し、エラーを特定する方法を説明します。 詳細とコードサンプルについては、[&#x200B; メッシュの作成 &#x200B;](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"} を参照してください。

## このビデオの目的は誰ですか。

* API メッシュを初めて使用するユーザー
* 複数の API ソースとGraphQL ソースの組み合わせに関心のある開発者

## ビデオコンテンツ

* [&#x200B; 変換 &#x200B;](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} を使用したデフォルトのソーススキーマの変更方法
* 名前の競合、スキーマの可用性、その他のスキーマ構文の問題などのエラーのトラブルシューティング方法
* 変更した設定でメッシュを更新する

>[!VIDEO](https://video.tv.adobe.com/v/3419789?quality=12&learn=on&captions=jpn)

## Json 設定ファイルを作成します。

API メッシュでは、JSON 設定ファイルを使用してソースハンドラーを定義します。 JSON ファイルには、メッシュのソースを含む `sources` 配列が含まれています。 複数のソースを持つメッシュの例を次に示します。

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
