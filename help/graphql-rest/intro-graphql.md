---
title: GraphQLはじめに
description: Adobe CommerceでのGraphQLの使用方法と [!DNL Magento Open Source]. Adobe Commerceおよび [!DNL Magento Open Source].
landing-page-description: Adobe CommerceでのGraphQLの使用方法と [!DNL Magento Open Source]. Adobe Commerceおよび [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
source-git-commit: 9dc530107470617f88992d8eb2ed9feb017a6530
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# GraphQLはじめに

GraphQLは、クライアント側アプリケーションがバックエンドと通信する強力さを実現するための業界標準にすぐになっています。 プラットフォームはヘッドレス実装の領域で機能を拡大し続けるので、Adobe Commerce開発者にとっては、ますます関連性の高いトピックです。

GraphQLを初めて使用する場合は、この節で基本的な概念と使用方法について説明します。

## GraphQLとは

GraphQLは、一意の API クエリ言語と、そのクエリ言語に応じてデータを提供するランタイムの仕様です。

REST などの従来の Web API は、データを送受信する複数の異なるシステムに対して適切に機能してきましたが、Progressive Web Applicationなどの最新のアプリリンクエクスペリエンスでは、ピークパフォーマンスよりも低くなっています。 このようなアプリケーションでは、 _同じ_ アプリケーションは Web API を介して通信します。 REST のようなスキームの連隊化アプローチは、多くの場合、このコンテキストで適切な柔軟性を提供しません。多くの種類のデータを迅速に取得する必要があります。

GraphQLは、お客様が表現的に _正確に_ 必要なデータ。 複数のデータ型を取得するために複数のネットワークリクエストを必要とする代わりに、1 つのリクエストで様々な型のクエリを実行できます。 また、（直感的にクエリをミラー化する形式で）要求されたタイプとフィールドのみを含めることで、応答がリーンに保たれます。

GraphQL仕様を実装するランタイムは、どの言語でも構築できます。 Adobe Commerceと [!DNL Magento Open Source] を使用します。
[graphql-php](https://webonyx.github.io/graphql-php/) PHP の実装を行い、その上に独自のレイヤーを構築します。

[完全なGraphQLドキュメントを表示](https://graphql.org/learn)

## GraphQLクライアントの使用

コードサンプルとチュートリアルをテストするには、GUI GraphQLクライアントが必要です。 次のオプションがあります。

* [アルテアール](https://altairgraphql.dev/) は、GraphQL専用に構築された優れた完全な機能を備えたクライアントです。 Adobeは、ウォークスルービデオで Altair を使用しています。
* デスクトップアプリケーションをインストールしない場合は、
   [クロム](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja)、Firefox、または [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa) ブラウザー。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql) は、GraphQL Foundation からのGraphQL IDE の実装です。 これはインストール可能なツールではなく、自分でインターフェイスを構築するために使用できるパッケージです。
* 既に [Postman](https://www.postman.com/)を使用する場合、GraphQLクエリは十分にサポートされますが、専用のGraphQLクライアントほど機能が完全には備わっていません。

GraphQLクライアントで、要求を URL パスに送信する必要があります。 `/graphql` Adobe Commerceまたは [!DNL Magento Open Source] インスタンス。 テストに既存のインスタンスを使用する場合は、Venia テーマのデモ (PWA Studioの実装例 ) を使用できます。 `https://venia.magento.com/graphql`
