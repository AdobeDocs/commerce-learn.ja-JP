---
title: GraphQLの概要
description: Adobe Commerceおよび [!DNL Magento Open Source]でGraphQLを使用する方法について説明します。 Adobe Commerceおよび [!DNL Magento Open Source]に対するGraphQL GETおよびPOST呼び出しを使用します。
short-description: Adobe Commerceと [!DNL Magento Open Source]に対するGraphQL GETとPOST呼び出しの使用方法について説明します。
kt: 11524
doc-type: video
duration: 286
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# GraphQLの概要

これは、GraphQLとAdobe Commerceのシリーズの第1部です。 GraphQLは、強力なクライアントサイドアプリケーションとバックエンドの連携を実現する上で、急速に業界標準となりつつあります。 Adobe Commerceの開発者にとって、ヘッドレス CMSの能力が引き続き重要な課題となっています。

GraphQLを初めて使用する場合は、この節では基本的な概念と使用方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3443941?captions=jpn&learn=on)

## このシリーズのGraphQLに関する関連動画とチュートリアル

* [パート 2 GraphQL - クエリ](../graphql-rest/graphql-queries.md)
* [第3部GraphQL – 変異](../graphql-rest/graphql-mutations.md)
* [&#x200B; パート 4 GraphQL - スキーマ &#x200B;](../graphql-rest/graphql-schema.md)

## GraphQLとは？

GraphQLは、一意のAPI クエリ言語と、そのクエリ言語に応じてデータを提供するランタイムの仕様です。

RESTのような従来のweb APIは、異なるシステムがデータをやり取りする際に適していますが、プログレッシブ web アプリケーションのような最新のアプリリンクエクスペリエンスでは、パフォーマンスのピーク時よりも少ない機能を提供しました。 このようなアプリケーションでは、_same_ アプリケーションのフロントエンドとバックエンドのレイヤーがweb APIを介して通信します。 RESTのようなスキームの規則化されたアプローチは、多くの種類のデータを迅速に取得する必要があるこの文脈では、適切な柔軟性を提供しないことが多い。

GraphQLを使用すると、クライアントは必要なデータを&#x200B;_正確に_&#x200B;記述できます。 複数のデータタイプを取得するために複数のネットワークリクエストを必要とする代わりに、1つのリクエストで多くのタイプに対してクエリを実行できます。 また、クエリを直感的に反映した形式で、要求されたタイプとフィールドのみを含めることで、応答はリーンに保たれます。

GraphQL仕様を実装するランタイムは、任意の言語で構築できます。 Adobe Commerceと[!DNL Magento Open Source]は
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP実装とその上に独自のレイヤーを構築します。

[GraphQLのドキュメント全体を見る](https://graphql.org/learn){target="_blank"}

## GraphQL クライアントの使用

コードサンプルとチュートリアルをテストするには、GUI GraphQL クライアントが必要です。 オプションはいくつかあります。

* [Altair](https://altairgraphql.dev/){target="_blank"}は、GraphQL専用に構築された優れた機能性を備えたクライアントです。 Adobeは、Altairをウォークスルー動画に使用しています。
* デスクトップアプリケーションをインストールしたくない場合は、Altairの拡張機能が
  [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}、Firefox、または[Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} ブラウザー。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"}は、GraphQL FoundationのGraphQL IDEの実装です。 これはインストール可能なツールではなく、自分でインターフェイスを構築するために使用できるパッケージです。
* [Postman](https://www.postman.com/){target="_blank"}に慣れている方は、GraphQL クエリの適切なサポートを利用できますが、専用のGraphQL クライアントほど十分な機能を備えていません。

GraphQL クライアントで、Adobe Commerceまたは`/graphql` インスタンスのURL パス [!DNL Magento Open Source]にリクエストを送信する必要があります。 テストに既存のインスタンスを使用する場合は、Venia テーマのデモ（PWA Studioの実装例）を使用できます。`https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
