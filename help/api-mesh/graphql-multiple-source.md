---
title: API メッシュで使用する複数のソースGraphQLを作成する
description: Adobe Commerceで API メッシュに複数のソースを使用する方法と [!DNL Adobe App Builder]. 一般的なエラーとその解決方法について説明します。
landing-page-description: Adobe Commerce と  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。複数のソースを持つメッシュの作成と、一般的なエラーの解決方法について説明します。
short-description: Adobe Commerce と  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。複数のソースを持つメッシュの作成と、一般的なエラーの解決方法について説明します。
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
source-wordcount: '243'
ht-degree: 8%

---

# 複数のソースを持つメッシュを作成する

このビデオでは、Adobe Developer App Builder 用の API メッシュで複数のソースを使用してメッシュを作成する方法を開発者が理解できます。 このビデオでは、複数のソースを持つメッシュを作成し、エラーを識別する方法を示します。 詳細およびコードサンプルについては、 [メッシュを作成する](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## このビデオは誰のためのものですか？

* API メッシュを初めて使用するユーザー
* 複数の API とGraphQLソースの組み合わせに関心のある開発者

## ビデオコンテンツ

* 使用方法 [変換](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} デフォルトのソーススキーマを変更するには
* 名前の競合、スキーマの可用性、その他のスキーマ構文の問題など、エラーのトラブルシューティング方法
* 変更した設定でメッシュを更新する

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## json 設定ファイルの作成

API Mesh では、JSON 設定ファイルを使用してソースハンドラーを定義します。 JSON ファイルには、 `sources` メッシュのソースを含む配列。 複数のソースを持つメッシュの例を次に示します。

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
