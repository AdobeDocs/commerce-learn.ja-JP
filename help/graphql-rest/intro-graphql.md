---
title: GraphQLはじめに
description: Adobe CommerceでのGraphQLの使用方法と [!DNL Magento Open Source]. Adobe CommerceおよびのGraphQLGETおよびPOST呼び出しの使用 [!DNL Magento Open Source].
short-description: Adobe Commerceおよび [!DNL Magento Open Source].
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: 750c8c9c5c6b3e01b9af8aacae31f3d521c4f7b7
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 0%

---

# GraphQLはじめに

これは、GraphQLとAdobe Commerceのシリーズの一部です。 GraphQLは、クライアント側アプリケーションがバックエンドと通信する強力さを実現するための業界標準にすぐになっています。 プラットフォームはヘッドレス実装の領域で機能を拡大し続けるので、Adobe Commerce開発者にとっては、ますます関連性の高いトピックです。

GraphQLを初めて使用する場合は、この節で基本的な概念と使用方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3424117?learn=on)

## このシリーズのGraphQLに関する関連ビデオとチュートリアル

* [第 2 部GraphQL — クエリ](../graphql-rest/graphql-queries.md)
* [第 3 部GraphQL — 突然変異](../graphql-rest/graphql-mutations.md)
* [第 4 部GraphQL — スキーマ](../graphql-rest/graphql-schema.md)

## GraphQLとは？

GraphQLは、一意の API クエリ言語と、そのクエリ言語に応じてデータを提供するランタイムの仕様です。

REST などの従来の Web API は、データを送受信する複数の異なるシステムに対して適切に機能してきましたが、Progressive Web Applicationなどの最新のアプリリンクエクスペリエンスでは、ピークパフォーマンスよりも低くなっています。 このようなアプリケーションでは、 _同じ_ アプリケーションは Web API を介して通信します。 REST のようなスキームの連隊化アプローチは、多くの場合、このコンテキストで適切な柔軟性を提供しません。多くの種類のデータを迅速に取得する必要があります。

GraphQLは、クライアントが表現的に説明することを可能にします _正確に_ 必要なデータ。 複数のデータ型を取得するために複数のネットワークリクエストを必要とする代わりに、1 つのリクエストで様々な型のクエリを実行できます。 また、（直感的にクエリをミラー化する形式で）要求されたタイプとフィールドのみを含めることで、応答がリーンに保たれます。

GraphQL仕様を実装するランタイムは、どの言語でも構築できます。 Adobe Commerceと [!DNL Magento Open Source] を使用します。
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP の実装を行い、その上に独自のレイヤーを構築します。

[完全なGraphQLドキュメントを表示](https://graphql.org/learn){target="_blank"}

## GraphQLクライアントの使用

コードサンプルとチュートリアルをテストするには、GUI GraphQLクライアントが必要です。 次のオプションがあります。

* [アルテアール](https://altairgraphql.dev/){target="_blank"} は、GraphQL専用に構築された優れた完全な機能を備えたクライアントです。 Adobeは、ウォークスルービデオで Altair を使用しています。
* デスクトップアプリケーションをインストールしない場合は、
  [クロム](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} ブラウザー。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} は、GraphQL Foundation からのGraphQL IDE の実装です。 これはインストール可能なツールではなく、自分でインターフェイスを構築するために使用できるパッケージです。
* 既に [Postman](https://www.postman.com/){target="_blank"}を使用する場合、GraphQLクエリは十分にサポートされますが、専用のGraphQLクライアントほど機能が完全には備わっていません。

GraphQLクライアントで、要求を URL パスに送信する必要があります。 `/graphql` Adobe Commerceまたは [!DNL Magento Open Source] インスタンス。 テストに既存のインスタンスを使用する場合は、Venia テーマのデモ (PWA Studioの実装例 ) を使用できます。 `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
