---
title: API メッシュの概要
description: Adobe Commerce と  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。Adobe App Builder のインストール、プロジェクトの操作、GraphQL リバースプロキシの作成などについて説明します。
landing-page-description: Adobe Commerce と  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。Adobe IO のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
short-description: Adobe Commerce と  [!DNL Adobe App Builder] で API メッシュを使用する方法について説明します。Adobe IO のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
kt: 11802
doc-type: tutorial
audience: all
last-substantial-update: 2023-8-28
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: baae6dab-48a4-49a0-b6f6-61cbebe63d0f
source-git-commit: 2ad0ae2aa7c9c852d300453f27f1be906976d95e
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 23%

---

# API メッシュの概要

Adobe Developer App Builder の API Mesh を初めて使用する場合は、他のビデオやチュートリアルに進む前に、この入門チュートリアルから始めることをお勧めします。

## API メッシュとは

API メッシュは、複数のデータソースを組み合わせて、アプリケーションが使用する単一の応答を取得します。

[API メッシュの完全なドキュメントを参照してください](https://developer.adobe.com/graphql-mesh-gateway/gateway/overview/){target="_blank"}

## このビデオは誰のためのものですか？

* API Mesh を初めて使用する開発者や [!DNL Adobe Commerce] ～を使った経験が限られている [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} と API メッシュ。

## ビデオコンテンツ

* API メッシュの概要
* 補足的なドキュメントへのリンク
* チェックアウト時にリアルタイムで在庫チェックをおこなう使用例
* コマースアプリケーションからの開発作業とリソース使用量の移動

>[!VIDEO](https://video.tv.adobe.com/v/3417534?quality=12&learn=on)

## 使用例

コマースアプリケーションには REST API とGraphQLエンドポイントがあります。 例えば、REST API を使用して特別な価格設定を適用したり、GraphQLエンドポイントを使用して在庫ステータスを処理したりできます。 API Mesh を使用すると、両方のエンドポイントを定義し、情報を取得し、1 回の応答としてリクエスト元のアプリケーションに返すことができます。

## リバースプロキシとは

App Builder と API Mesh を使用するAdobe者は、リバースプロキシとは何かを理解する必要はありません。 ただし、AdobeApp Builder に関連する全体的な機能に興味がある場合は、次のリソースを使用してください。

* [リバースプロキシとは](https://www.imperva.com/learn/performance/reverse-proxy/){target="_blank"}
* [リバースプロキシとは何かと、なぜ重要なのか](https://blog.hubspot.com/website/reverse-proxy){target="_blank"}

{{$include /help/_includes/api-mesh-related-links.md}}
