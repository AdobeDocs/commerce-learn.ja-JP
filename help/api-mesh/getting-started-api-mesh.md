---
title: API メッシュの基本を学ぶ
description: Adobe Commerceで API メッシュを使用する方法と [!DNL Adobe App Builder]. Adobe App Builder のインストール、プロジェクトの操作、GraphQL リバースプロキシの作成などについて説明します。
landing-page-description: Adobe Commerceで API メッシュを使用する方法と [!DNL Adobe App Builder]. Adobe IO のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
short-description: Adobe Commerceで API メッシュを使用する方法と [!DNL Adobe App Builder]. Adobe IO のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
kt: 11802
doc-type: tutorial
audience: all
last-substantial-update: 2023-8-27
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: baae6dab-48a4-49a0-b6f6-61cbebe63d0f
source-git-commit: 366a7988dfa1de39ebccb8ab0e281d80b27dbb36
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 15%

---

# API メッシュの基本を学ぶ

Adobe Developer Adobe App Builder の API メッシュを初めて使用する場合は、他のビデオやチュートリアルに進む前に、この入門チュートリアルから始めることをお勧めします。

## API メッシュとは

API メッシュは、複数のデータソースを組み合わせて、アプリケーションが使用する 1 つの応答を取得します。

[完全な API メッシュのドキュメントを表示](https://developer.adobe.com/graphql-mesh-gateway/gateway/overview/){target="_blank"}

## このビデオの目的は誰ですか。

* API メッシュを初めて使用する開発者、または [!DNL Adobe Commerce] の使用経験が限られているユーザー [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} と API メッシュです。

## ビデオコンテンツ

* API メッシュの概要
* 補足ドキュメントへのリンク
* チェックアウト時にリアルタイムで在庫チェックを行うユースケース
* 開発作業とリソース使用状況をコマースアプリケーションから移動する

>[!VIDEO](https://video.tv.adobe.com/v/3417534?quality=12&learn=on)

## 使用例

Commerce アプリケーションには、REST API とGraphQL エンドポイントがあります。 例えば、REST API を使用して特別な価格を適用したり、GraphQL エンドポイントを使用して在庫ステータスを処理したりできます。 API メッシュを使用すると、両方のエンドポイントを定義し、情報を取得して、1 つの応答として要求元のアプリケーションに返すことができます。

## リバースプロキシとは

Adobeの App Builder と API メッシュを使用するデベロッパーの場合、リバースプロキシとは何かを理解する必要はありません。 ただし、Adobeの App Builder に関連する全体的な機能に興味がある場合は、次の資料を使用してください。

* [リバースプロキシとは](https://www.imperva.com/learn/performance/reverse-proxy/){target="_blank"}
* [リバースプロキシとは何か、そしてそれが重要な理由](https://blog.hubspot.com/website/reverse-proxy){target="_blank"}

{{$include /help/_includes/api-mesh-related-links.md}}
