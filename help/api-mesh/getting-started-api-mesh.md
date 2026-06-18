---
title: API Meshの基本を学ぶ
description: Adobe CommerceとAdobe App BuilderでAPI Meshを使用する方法（App Builderのインストール、プロジェクトの操作、リバースプロキシの作成など）について説明します。
jira: KT-11802
doc-type: Tutorial
duration: 422
last-substantial-update: 2023-08-27T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Intermediate
exl-id: baae6dab-48a4-49a0-b6f6-61cbebe63d0f
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# API Meshの基本を学ぶ

Adobe Developer App Builder用API Meshを初めて使用する場合は、他のビデオやチュートリアルに進む前に、この入門チュートリアルから始めることをAdobeにお勧めします。

## API Meshとは

API Meshは、複数のデータソースを組み合わせて、アプリケーションが使用するための単一の応答を取得します。

[API Meshのドキュメントを見る](https://developer.adobe.com/graphql-mesh-gateway/mesh/){target="_blank"}

## この動画は誰のためのものでしょうか？

* [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"}およびAPI Meshを使用した経験が限られているAPI Meshまたは[!DNL Adobe Commerce]を初めて使用する開発者。

## ビデオコンテンツ

* API Meshの概要
* 補足ドキュメントへのリンク
* チェックアウト時にリアルタイムで在庫を確認するユースケース
* 開発作業やリソース使用をadobe commerce アプリケーションから移行

>[!VIDEO](https://video.tv.adobe.com/v/3421888?captions=jpn&learn=on)

## 使用例

Commerce アプリケーションにはREST APIとGraphQL エンドポイントがあります。 たとえば、REST APIを使用して特別価格を適用したり、GraphQLエンドポイントを使用して在庫状況を処理したりします。 API Meshを使用すると、両方のエンドポイントを定義して情報を取得し、1つの応答としてリクエスト側のアプリケーションに返すことができます。

## リバースプロキシとは

Adobe App BuilderとAPI Meshを使用する開発者は、リバースプロキシの定義を理解する必要はありません。 ただし、Adobe App Builderに関連する機能全体に関心がある場合は、次の資料を参照してください。

* [リバースプロキシとは](https://www.imperva.com/learn/performance/reverse-proxy/){target="_blank"}


{{$include /help/_includes/api-mesh-related-links.md}}
