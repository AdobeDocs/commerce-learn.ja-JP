---
title: GraphQLを使用してクエリを実行します
description: Adobe CommerceでGraphQLを使用してクエリを実行し、 [!DNL Magento Open Source]. これは、GETとPOST呼び出しを使用したGraphQLの概要です。
landing-page-description: Adobe CommerceでGraphQLを使用してクエリを実行し、 [!DNL Magento Open Source]. これは、GETとPOST呼び出しを使用したGraphQLの概要です。
short-description: Adobe CommerceでGraphQLを使用してクエリを実行し、 [!DNL Magento Open Source]. これは、GETとPOST呼び出しを使用したGraphQLの概要です。
kt: 13937
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 0%

---

# GraphQL クエリ

これは、GraphQLとAdobe Commerceのシリーズの第 2 部です。 このチュートリアルとビデオでは、GraphQL クエリと、Adobe Commerceに対するクエリの実行方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3424120?learn=on)

## このシリーズのGraphQLに関する関連ビデオとチュートリアル

* [第 1 部GraphQL – はじめに](../graphql-rest/intro-graphql.md)
* [第 3 部GraphQL – 突然変異](../graphql-rest/graphql-mutations.md)
* [第 4 部GraphQL - スキーマ](../graphql-rest/graphql-schema.md)

## GraphQL構文の例

本格的な例で、GraphQLのクエリ構文について詳しく説明します。 （https://venia.magento.com/graphqlに対して自分で試すことができます）。

次のGraphQLのクエリが 1 つずつ分類されていることがわかります。

```graphql
{
    country (
        id: "US"
    ) {
        id
        full_name_english
    }

    categories(
        filters: {
            name: {
                match: "Tops"
            }
        }
    ) {
        items {
            name
            products(
                pageSize: 10,
                currentPage: 2
            ) {
                items {
                    sku
                }
            }
        }
    }
}
```

上記のクエリに対して、GraphQL サーバーから妥当な応答は、次のようになります。

```json
{
  "data": {
    "country": {
      "id": "US",
      "full_name_english": "United States"
    },
    "categories": {
      "items": [
        {
          "name": "Tops",
          "products": {
            "items": [
              {
                "sku": "VSW06"
              },
              {
                "sku": "VT06"
              },
              {
                "sku": "VSW07"
              },
              {
                "sku": "VT07"
              },
              {
                "sku": "VSW08"
              },
              {
                "sku": "VT08"
              },
              {
                "sku": "VSW09"
              },
              {
                "sku": "VT09"
              },
              {
                "sku": "VSW10"
              },
              {
                "sku": "VT10"
              }
            ]
          }
        }
      ]
    }
  }
}
```

上記の例は、サーバーで定義された、Adobe Commerceの標準GraphQL スキーマに基づいています。 このリクエストでは、複数のタイプのデータを一度にクエリします。 クエリは、必要なフィールドを正確に表し、返されるデータはクエリ自体と同じような形式になります。

>[!NOTE]
>
>GraphQL クライアントは、送信される実際の HTTP リクエストの形式を難読化しますが、これは簡単に見つけることができます。 ブラウザーベースのクライアントを使用している場合は、 [!UICONTROL Network] クエリが送信されたら Tab キーを押します。 リクエストに、「query: `{string}`」と入力します。 `{string}` は、クエリ全体の生の文字列にすぎません。 リクエストがGETとして送信される場合、クエリは、代わりにクエリ文字列パラメーター「query」でエンコードされる可能性があります。 REST の場合とは異なり、HTTP リクエストタイプは関係なく、クエリのコンテンツだけです。


## 必要なもののクエリ

`country` および `categories` この例では、2 種類のデータ用に 2 つの異なる「クエリ」を表しています。 データタイプごとに個別の明示的なエンドポイントを定義する REST などの従来の API パラダイムとは異なります。 GraphQLでは、1 つのエンドポイントに式を使用してクエリを実行し、様々な種類のデータを一度に取得できます。

同様に、クエリは、両方に必要なフィールドを正確に指定します `country` （`id` および `full_name_english`）および `categories` （`items`。それ自体はフィールドのサブ選択を持ちます）。受け取ったデータは、そのフィールドの仕様を反映しています。 これらのデータタイプには、さらに多くのフィールドを使用できると考えられますが、取得できるのは要求した内容のみです。


>[!NOTE]
>
>の戻り値 `items` は、実際には次のとおりです _配列_ の値ですが、そのためのサブフィールドを直接選択しています。 フィールドのタイプがリストの場合、GraphQLはリスト内の各項目に適用するサブ選択を暗黙的に認識します。

## 引数

返すフィールドは各型の中括弧で囲んで指定しますが、名前付きの引数とその値は、型名の後の括弧内で指定します。 引数は多くの場合オプションで、クエリ結果のフィルタリング、書式設定、その他の変換の方法に影響を与えます。

を渡しています `id` ～に対する議論 `country`、問い合わせる特定の国を指定し、 `filters` 対する議論 `categories`.

## 下の方のフィールド

あなたが考える傾向がある間 `country` および `categories` 個別のクエリまたはエンティティとして、クエリで表現されるツリー全体は、実際にはフィールドのみで構成されます。 式 `products` 構文上は～と全く変わらない `categories`. どちらもフィールドで、構造に違いはありません。

すべてのGraphQL データグラフは、1 つの「ルート」タイプ（通常は `Query`）を選択してツリーを開始します。通常、エンティティと見なされるタイプは、このルートのフィールドに割り当てられます。 サンプルクエリでは、実際にルートタイプに対して 1 つの汎用クエリを作成し、フィールドを選択します `country` および `categories`. その後、これらのフィールドのサブフィールドを選択し、潜在的に複数のレベルで選択します。 フィールドの戻り値の型が複合型である場合（たとえば、スカラー型ではなく独自のフィールドを持つ場合）は、引き続き目的のフィールドを選択します。

ネストされたフィールドのこの概念は、に引数を渡すことができる理由でもあります `products` （`pageSize` および `currentPage`）を選択します `categories` フィールド。

![GraphQL フィールドツリー](../assets/graphql-field-tree.png)

## 変数

別のクエリを試してみましょう。

```graphql
query getProducts(
    $search: String
) {
    products(
        search: $search
    ) {
        items {
            ...productDetails
            related_products {
                ...productDetails
            }
        }
    }
}

fragment productDetails on ProductInterface {
    sku
    name
}
```

まず注意すべきは、キーワードが追加されたことです `query` クエリの左中括弧の前に、操作名（`getProducts`）に設定します。 この操作名は任意です。サーバースキーマ内の値には対応しません。 この構文は、変数の導入をサポートするために追加されました。

前のクエリでは、フィールドの引数の値を文字列または整数として直接ハードコーディングしました。 ただし、GraphQLの仕様には、変数を使用してメインクエリからユーザー入力を分離するためのファーストクラスのサポートがあります。

新しいクエリでは、クエリ全体の左中括弧の前にかっこを付けて、 `$search` 変数（変数には常にドル記号のプレフィックス構文を使用します）。 に提供されているのは、この変数です。 `search` 対する議論 `products`.

クエリに変数が含まれる場合、GraphQL リクエストには、JSON でエンコードされた個別の値ディクショナリがクエリ自体と一緒に含まれる必要があります。 上記のクエリの場合、クエリ本文に加えて、変数値の次の JSON を送信する場合があります。

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>独自のAdobe Commerce インスタンスではなく、Venia サンプルサイトに対してこれらのクエリを実行すると、返される結果は空になる可能性があります。 `related_products`.

テストに使用するGraphQL対応のクライアント（Altair や GraphiQL など）では、UI は、クエリとは別に変数 JSON を入力することをサポートしています。

GraphQL クエリの実際の HTTP リクエストに「query: `{string}```本文内に変数ディクショナリを含むリクエストには、追加の``変数を含めるだけです。 `{json}`」と入力します。ここで、 `{json}` は、変数値を持つ JSON 文字列です。

新しいクエリでは、も使用します _フラグメント_ （`productDetails`）を選択して、同じフィールド選択を複数の場所で再利用します。 [フラグメントの詳細情報](https://graphql.org/learn/queries/#fragments){target="_blank"} GraphQLのドキュメントで説明しています。

{{$include /help/_includes/graphql-rest-related-links.md}}
