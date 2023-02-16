---
title: API メッシュの概要
description: Adobe Commerceおよび [!DNL Adobe App Builder]. AdobeApp Builder のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
landing-page-description: Adobe Commerceおよび [!DNL Adobe App Builder]. AdobeIO のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
kt: 11802
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b3d5b22a597b342df6bf9846346d656dd4ce1383
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# API メッシュの概要

Adobe Developer App Builder の API メッシュを初めて使用する場合は、他のビデオやチュートリアルに進む前に、この入門チュートリアルから始めることをお勧めします。

## API メッシュとは

API メッシュは、複数のデータソースを組み合わせて、アプリケーションが使用する単一の応答を取得します。

[API メッシュの完全なドキュメントを参照してください](https://developer.adobe.com/graphql-mesh-gateway/gateway/overview/)

## 使用例

コマースアプリケーションには REST API とGraphQLエンドポイントがあります。 例えば、REST API を使用して特別な価格設定を適用したり、GraphQLエンドポイントを使用して在庫ステータスを処理したりできます。 API Mesh を使用すると、両方のエンドポイントを定義し、情報を取得し、1 回の応答としてリクエスト元のアプリケーションに返すことができます。

## リバースプロキシとは

App Builder と API Mesh を使用するAdobe者は、リバースプロキシとは何かを理解する必要はありません。 ただし、AdobeApp Builder に関連する全体的な機能に興味がある場合は、次のリソースを使用してください。

* [リバースプロキシとは](https://www.imperva.com/learn/performance/reverse-proxy/)
* [リバースプロキシとは何かと、なぜ重要なのか](https://blog.hubspot.com/website/reverse-proxy)

{{$include /help/_includes/api-mesh-related-links.md}}
