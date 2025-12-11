---
title: GraphQLを使用してクエリを実行します
description: Adobe CommerceでGraphQLを使用してクエリを実行し、 [!DNL Magento Open Source] を実行する方法を説明します。 これは、GETと POST 呼び出しを使用したGraphQLの概要です。
landing-page-description: Adobe CommerceでGraphQLを使用してクエリを実行し、 [!DNL Magento Open Source] を実行する方法を説明します。 これは、GETと POST 呼び出しを使用したGraphQLの概要です。
short-description: Adobe CommerceでGraphQLを使用してクエリを実行し、 [!DNL Magento Open Source] を実行する方法を説明します。 これは、GETと POST 呼び出しを使用したGraphQLの概要です。
kt: 13937
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
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
* [&#x200B; 第 4 部GraphQL - スキーマ &#x200B;](../graphql-rest/graphql-schema.md)

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

上記の例は、サーバーで定義された、Adobe Commerceの標準GraphQL スキーマに基づいています。 このリクエストでは、次の操作を行います
複数タイプのデータを一度にクエリします。 クエリは、必要なフィールドを正確に表し、返されるデータは書式設定されます
クエリ自体と同様です。

>[!NOTE]
>
>GraphQL クライアントは、送信される実際の HTTP リクエストの形式を難読化しますが、これは簡単に見つけることができます。 ブラウザーベースのクライアントを使用している場合は、クエリの送信時に「[!UICONTROL Network]」タブを確認します。 リクエストに、「query:`{string}`」で構成される生の本文が含まれていることがわかります。`{string}` は、単にクエリ全体の生の文字列です。 リクエストがGETとして送信される場合、クエリは、代わりにクエリ文字列パラメーター「query」でエンコードされる可能性があります。 REST の場合とは異なり、HTTP リクエストタイプは関係なく、クエリのコンテンツだけです。


## 必要なもののクエリ

この例の `country` と `categories` は、2 種類のデータに対する 2 つの異なる「クエリ」を表しています。 データタイプごとに個別の明示的なエンドポイントを定義する REST などの従来の API パラダイムとは異なります。 GraphQLでは、1 つのエンドポイントに式を使用してクエリを実行し、様々な種類のデータを一度に取得できます。

同様に、クエリは `country` （`id` と `full_name_english`）および `categories` （`items` 自身はフィールドのサブ選択を持つ）の両方に必要なフィールドを正確に指定し、受け取るデータはそのフィールドの指定を反映します。 これらのデータタイプには、さらに多くのフィールドを使用できると考えられますが、取得できるのは要求した内容のみです。


>[!NOTE]
>
>`items` の戻り値は実際には値の _配列_ であることに気づくかもしれませんが、それでもサブフィールドを直接選択しています。 フィールドのタイプがリストの場合、GraphQLはリスト内の各項目に適用するサブ選択を暗黙的に認識します。

## 引数

返すフィールドは各型の中括弧で囲んで指定しますが、名前付きの引数とその値は、型名の後の括弧内で指定します。 引数は多くの場合オプションで、クエリ結果のフィルタリング、書式設定、その他の変換の方法に影響を与えます。

`id` 引数を `country` に渡し、クエリする特定の国を指定し、`filters` に `categories` 引数を指定しています。

## 下の方のフィールド

`country` と `categories` は別のクエリやエンティティと考えられがちですが、クエリで表現されるツリー全体は、実際にはフィールドのみで構成されています。 `products` の表現は構文上、`categories` の表現と全く変わらない。 どちらもフィールドで、構造に違いはありません。

どのGraphQL データグラフにも、ツリーを開始するための 1 つの「ルート」タイプ（通常 `Query` と呼ばれます）があり、エンティティと見なされることが多いタイプは、このルートのフィールドに割り当てられます。 例のクエリは、実際にはルートタイプに対して 1 つの汎用クエリを作成し、`country` フィールドと `categories` フィールドを選択しています。 その後、これらのフィールドのサブフィールドを選択し、潜在的に複数のレベルで選択します。 フィールドの戻り値の型が複合型である場合（たとえば、スカラー型ではなく独自のフィールドを持つ場合）は、引き続き目的のフィールドを選択します。

また、ネストされたフィールドのこの概念は、最上位の `products` フィールドに対して行ったのと同じ方法で `pageSize` （`currentPage` および `categories`）の引数を渡すことができる理由でもあります。

![GraphQL フィールドツリー &#x200B;](../assets/graphql-field-tree.png)

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

まず注意すべきは、クエリの左中括弧の前にキーワード `query` と操作名（`getProducts`）が追加されたことです。 この操作名は任意です。サーバースキーマ内の値には対応しません。 この構文は、変数の導入をサポートするために追加されました。

前のクエリでは、フィールドの引数の値を文字列または整数として直接ハードコーディングしました。 ただし、GraphQLの仕様には、変数を使用してメインクエリからユーザー入力を分離するためのファーストクラスのサポートがあります。

新しいクエリでは、クエリ全体の左中括弧の前に括弧を使用して、`$search` 変数を定義します（変数は常にドル記号のプレフィックス構文を使用します）。 `search` の `products` 引数に指定されているのは、この変数です。

クエリに変数が含まれる場合、GraphQL リクエストには、JSON でエンコードされた個別の値ディクショナリがクエリ自体と一緒に含まれる必要があります。 上記のクエリの場合、クエリ本文に加えて、変数値の次の JSON を送信する場合があります。

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>独自のAdobe Commerce インスタンスではなく Venia サンプルサイトに対してこれらのクエリを実行すると、`related_products` では返される結果が空の可能性があります。

テストに使用するGraphQL対応のクライアント（Altair や GraphiQL など）では、UI は、クエリとは別に変数 JSON を入力することをサポートしています。

GraphQL クエリの実際の HTTP リクエストの本文に「query: `{string}`」が含まれるのと同じように、変数ディクショナリを含むリクエストには、同じ本文に追加の「variables: `{json}`」が含まれます。`{json}` は変数値を持つ JSON 文字列です。

また、新しいクエリでは _フラグメント_ （`productDetails`）を使用して、同じフィールド選択を複数の場所で再利用します。 [&#x200B; フラグメントについて詳しくは &#x200B;](https://graphql.org/learn/queries/#fragments){target="_blank"} GraphQL ドキュメントを参照してください。

{{$include /help/_includes/graphql-rest-related-links.md}}
