---
title: API Meshの基本を学ぶ
description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshを使用する方法について説明します。 Adobe App Builder のインストール、プロジェクトの操作、GraphQL リバースプロキシの作成などについて説明します。
landing-page-description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshを使用する方法について説明します。 Adobe IO のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
short-description: Adobe Commerceおよび [!DNL Adobe App Builder]でAPI Meshを使用する方法について説明します。 Adobe IO のインストール、プロジェクトの操作、graphql リバースプロキシの作成などについて説明します。
kt: 11802
doc-type: tutorial
duration: 442
audience: all
last-substantial-update: 2023-8-27
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: baae6dab-48a4-49a0-b6f6-61cbebe63d0f
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 16%

---

# API Meshの基本を学ぶ

Adobe Developer App Builder用API Meshを初めて使用する場合は、他のビデオやチュートリアルに進む前に、この入門チュートリアルから始めることをAdobeにお勧めします。

## API Meshとは

API Meshは、複数のデータソースを組み合わせて、アプリケーションが使用するための単一の応答を取得します。

[完全なAPI Mesh ドキュメントを表示](https://developer.adobe.com/graphql-mesh-gateway/gateway/overview/){target="_blank"}

## この動画は誰のためのものでしょうか？

* [!DNL Adobe Commerce]Adobe I/O Runtime[およびAPI Meshを使用した経験が限られているAPI Meshまたは](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"}を初めて使用する開発者。

## ビデオコンテンツ

* API Meshの概要
* 補足資料へのリンク
* チェックアウト時にリアルタイムで在庫を確認するユースケース
* 開発作業やリソース使用をadobe commerce アプリケーションから移行

>[!VIDEO](https://video.tv.adobe.com/v/3417534?learn=on)

## 使用例

Commerce アプリケーションにはREST APIとGraphQL エンドポイントがあります。 たとえば、REST APIを使用して特別価格を適用したり、GraphQLエンドポイントを使用して在庫状況を処理したりできます。 API Meshを使用すると、両方のエンドポイントを定義して情報を取得し、1つの応答としてリクエスト側のアプリケーションに返すことができます。

## リバースプロキシとは

Adobe App BuilderとAPI Meshを使用する開発者は、リバースプロキシとは何かを理解する必要はありません。 ただし、Adobe App Builderに関連する機能全体に関心がある場合は、次の資料を参照してください。

* [&#x200B; リバースプロキシとは](https://www.imperva.com/learn/performance/reverse-proxy/){target="_blank"}


{{$include /help/_includes/api-mesh-related-links.md}}
