---
title: GraphQLの概要
description: Adobe CommerceでのGraphQLの使用方法と詳細 [!DNL Magento Open Source]. Adobe Commerceに対してGraphQL GETおよびPOST呼び出しを使用し、 [!DNL Magento Open Source].
short-description: Adobe CommerceとのGraphQL GETおよびPOST呼び出しの使用方法を説明します [!DNL Magento Open Source].
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: b8b1e40a2f4d38954f0d21bc6f1a91b7ec0bd8c9
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# GraphQLの概要

これは、GraphQLとAdobe Commerceのシリーズの第 1 部です。 GraphQLは、クライアントサイドの強力なアプリケーションがバックエンドとどのように通信するかという点で、すぐに業界標準になりました。 ヘッドレス実装の分野でプラットフォームの機能が拡張され続けているため、Adobe Commerce開発者にとってますます関連性の高いトピックとなっています。

GraphQLを初めて使用する場合は、この節で、基本的な概念と使用方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3424117?learn=on)

## このシリーズのGraphQLに関する関連ビデオとチュートリアル

* [第 2 部GraphQL - クエリ](../graphql-rest/graphql-queries.md)
* [第 3 部GraphQL – 突然変異](../graphql-rest/graphql-mutations.md)
* [第 4 部GraphQL - スキーマ](../graphql-rest/graphql-schema.md)

## GraphQLとは

GraphQLは、一意の API クエリ言語と、そのクエリ言語に応じてデータを提供するランタイムの仕様です。

REST などの従来の web API は、異なるシステム間でデータをやり取りするのに適していましたが、Progressive Web Applicationのような最新のアプリとリンクのエクスペリエンスではピーク時のパフォーマンスよりも少ない結果でした。 このようなアプリケーションでは、のフロントエンド層とバックエンド層 _同じ_ アプリケーションは web API を介して通信します。 REST などのスキームの制限付きアプローチでは、多くの種類のデータを迅速に取得する必要があるこのコンテキストでは、多くの場合、適切な柔軟性が提供されません。

GraphQLを使用すると、クライアントは以下を表現できます _正確に_ 必要なデータ。 複数のデータタイプを取得するために複数のネットワークリクエストを必要とする代わりに、1 つのリクエストで様々なタイプをクエリできます。 また、要求されたタイプとフィールドのみを（クエリを直感的に反映した形式で）含めることで、応答はリーン状態に保たれます。

GraphQLの仕様を実装するランタイムは、任意の言語で構築できます。 Adobe Commerceと [!DNL Magento Open Source] を使用する
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP の実装とその上に独自のレイヤーを構築します。

[完全なGraphQLのドキュメントを表示](https://graphql.org/learn){target="_blank"}

## GraphQL クライアントの使用

コードサンプルとチュートリアルをテストするには、GUI GraphQL クライアントが必要です。 次のいくつかのオプションがあります。

* [アルタイル](https://altairgraphql.dev/){target="_blank"} は、GraphQL向けに特別に構築された、優れた機能を備えたクライアントです。 Adobeでは、ウォークスルー動画で Altair を使用します。
* デスクトップアプリケーションをインストールしない場合は、で実行される Altair 拡張機能もあります
  [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}、Firefox または [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} ブラウザー。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} は、GraphQL財団からのGraphQL IDE の実装です。 これはインストール可能なツールではなく、インターフェイスを自分で構築するために使用できるパッケージです。
* 既に詳しい場合は、 [Postman](https://www.postman.com/){target="_blank"}専用のGraphQL クライアントほど完全には機能しませんが、GraphQLのクエリを適度にサポートしています。

GraphQL クライアントで、URL パスにリクエストを送信する必要があります `/graphql` Adobe Commerceまたは [!DNL Magento Open Source] インスタンス。 テストに既存のインスタンスを使用する場合は、Venia テーマのデモ（PWA Studioの実装例）を使用できます。 `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
