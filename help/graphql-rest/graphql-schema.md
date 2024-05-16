---
title: GraphQLを使用したスキーマ言語
description: GraphQLに関連するスキーマについて説明します。 スキーマの説明を、興味深いパターンやスキーマの読み方と共に読みます。
landing-page-description: GraphQLの概要を説明します。 スキーマの理解と一部の要素の解釈方法
short-description: GraphQLの概要を説明します。 スキーマの理解と一部の要素の解釈方法
kt: 13939
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---

# スキーマ言語

これは、GraphQLとAdobe Commerceのシリーズの第 4 部です。 使用されるクエリとミューテーションは、サーバーに実装されている特定のデータグラフに依存します。GraphQL ランタイムは、このデータグラフを使用してクエリを解決します。 GraphQLの仕様は、データグラフのタイプと関係を表すための非依存型の言語を定義します。

>[!VIDEO](https://video.tv.adobe.com/v/3424123?learn=on)

## このシリーズのGraphQLに関する関連ビデオとチュートリアル

* [第 1 部GraphQL – はじめに](../graphql-rest/intro-graphql.md)
* [第 2 部GraphQL - クエリ](../graphql-rest/graphql-queries.md)
* [第 3 部GraphQL – 突然変異](../graphql-rest/graphql-mutations.md)

## サンプルスキーマ

ここまでに見たクエリとミューテーションをサポートする、短縮されたタイプスキーマを次に示します。

```graphql
input FilterMatchTypeInput {
  match: String
}

type Money {
  value: Float
}

type Country {
  id: String
  full_name_english: String
}

interface ProductInterface {
  sku: String
  name: String
  related_products: [ProductInterface]
}

type CategoryFilterInput {
  name: FilterMatchTypeInput
}

type CategoryProducts {
  items: [ProductInterface]
}

type CategoryTree {
  name: String
  products(pageSize: Int, currentPage: Int): CategoryProducts
}

type CategoryResult {
  items: [CategoryTree]
}

type Products {
  items: [ProductInterface]
}

type Query {
  country (id: String): Country
  categories (filters: CategoryFilterInput): CategoryResult
  products (search: String): Products
}

input CartItemInput {
  sku: String!
  quantity: Float!
}

type CartPrices {
  grand_total: Money
}

type Cart {
  prices: CartPrices
  total_quantity: Float!
}

type AddProductsToCartOutput {
  cart: Cart!
}

type Mutation {
  addProductsToCart(cartId: String!, cartItems: [CartItemInput!]!): AddProductsToCartOutput
}
```

詳しくは、以下を参照してください。 [GraphQL ドキュメント](https://graphql.org/learn/schema/){target="_blank"} ここでは説明しない概念の構文など、タイプシステムの詳細について説明します。 ただし、上記の例は自明です。 （また、構文がクエリ構文とどのように似ているかに注意してください。） GraphQL スキーマの定義は、特定のタイプの使用可能な引数とフィールド、およびそれらのフィールドのタイプを表現するだけです。 各複合フィールドタイプには、それ自体に定義が必要です。次のような単純なスカラータイプになるまで、ツリー全体に定義が必要になります。 `String`.

この `input` 宣言は、すべての点で A のようなものです `type` 引数の入力として使用できる型を定義します。 また、に注意してください `interface` 宣言。 これは PHP のインタフェースと同じかそれ以下の機能を提供します。 他のタイプは、このインターフェイスから継承します。

構文 `[CartItemInput!]!` 見た目はトリッキーですが、最終的にはかなり直感的です。 この `!` _内側_ 括弧は、配列内のすべての値を null 以外にする必要があることを宣言し、一方で _外部_ 配列値自体を null 以外にする必要があることを宣言します（例：空の配列）。

>[!NOTE]
>
>スキーマに従ってデータを取得してフォーマットする方法や、そのようなロジックを特定の型にマッピングする方法は、GraphQL ランタイムの実装次第です。 ただし、実装は、ネストされたフィールドを理解する上で意味のある概念的なフローに従う必要があります。ルートに関連付けられた解決操作です `Query` または `Mutation` タイプが実行され、リクエストで指定された各フィールドが調べられます。 複合タイプに解決されるフィールドごとに、そのタイプに対して同様の解決が行われ、すべてがスカラー値に解決されます。

{{$include /help/_includes/graphql-rest-related-links.md}}
