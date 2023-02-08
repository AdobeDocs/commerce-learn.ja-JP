---
title: API メッシュの概要
description: Adobe Commerceおよび [!DNL Adobe App Builder]. Adobe Developerのインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
landing-page-description: Adobe Commerceおよび [!DNL Adobe App Builder]. AdobeIO のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
kt: 11802
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# API メッシュの概要

API Mesh を初めて使用する場合は、付属のビデオとチュートリアルを参照する前に、この入門チュートリアルから始めることをお勧めします。

## API メッシュとは

API Mesh を使用すると、複数のデータソースを使用して、コマースアプリケーションが使用する単一の応答を提供できます。

[API メッシュの完全なドキュメントを参照してください](https://developer.adobe.com/graphql-mesh-gateway/gateway/overview/)

## 使用例

コマースアプリケーションのバックエンドシステムには、特別な価格を取得するために使用できる REST API があります。 GraphQLエンドポイントを持つ別のバックエンドシステムが在庫ステータスを処理します。 API Mesh を使用すると、両方のエンドポイントを定義し、情報を取得し、1 回の応答としてリクエスト元のアプリケーションに返すことができます。

## リバースプロキシとは

App Builder とGraphQL Mesh を使用するAdobe者は、リバースプロキシとは何かを理解する必要はありません。 ただし、AdobeApp Builder に関連する機能全体に興味を持つ場合があります。 インターネット上には多くの良いリソースが見つかる。
リバースプロキシの基本機能について詳しくは、以下に外部リソースを示します。

* [リバースプロキシとは](https://www.imperva.com/learn/performance/reverse-proxy/)
* [リバースプロキシとは何かと、なぜ重要なのか](https://blog.hubspot.com/website/reverse-proxy)

{{$include /help/_includes/api-mesh-related-links.md}}
