---
title: GraphQLのスキーマ言語
description: GraphQLのスキーマについて説明します。 スキーマの説明と、いくつかの興味深いパターンとスキーマの読み方を読みます。
landing-page-description: GraphQLの概要を説明します。 スキーマを理解し、一部の要素を解釈する方法
short-description: GraphQLの概要を説明します。 スキーマを理解し、一部の要素を解釈する方法
kt: 13939
doc-type: video
duration: 363
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---

# スキーマ言語

これはGraphQLとAdobe Commerceのシリーズの第4部です。 使用されるクエリと変異は、GraphQL ランタイムがクエリを解決するために使用する、サーバーに実装されている特定のデータグラフに依存しています。 GraphQL仕様では、データグラフの種類と関係を表現するための非依存の言語を定義します。

>[!VIDEO](https://video.tv.adobe.com/v/3424123?learn=on)

## このシリーズのGraphQLに関する関連動画とチュートリアル

* [第1部GraphQL – 概要](../graphql-rest/intro-graphql.md)
* [パート 2 GraphQL - クエリ](../graphql-rest/graphql-queries.md)
* [第3部GraphQL – 変異](../graphql-rest/graphql-mutations.md)

## スキーマの例

ここでは、これまで見てきたクエリや突然変異をサポートする、簡略化されたタイプスキーマを紹介します。

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

[GraphQL ドキュメント ](https://graphql.org/learn/schema/){target="_blank"}を詳しく調べて、ここで表されていない一部の概念の構文を含め、書体システムの詳細について学ぶことができます。 上記の例は自明である。 （また、構文がクエリ構文とどの程度似ているかに注意してください）。 GraphQL スキーマの定義は、特定の型の利用可能な引数とフィールドを、それらの型と共に表現するだけです。 `String`のような単純なスカラー型に到達するまで、各フィールド型はツリーを通して定義を持つ必要があります。

`input`宣言は、`type`のように、すべての点で有効ですが、引数の入力として使用できる型を定義します。 `interface`宣言にも注意してください。 これは、PHPのインターフェイスと多かれ少なかれ同じ関数を提供します。 その他のタイプはこのインターフェイスから継承されます。

構文`[CartItemInput!]!`は難しそうに見えますが、最終的にはかなり直感的です。 ブラケットの`!` _inside_&#x200B;は、配列内のすべての値がnullでないことを宣言し、一方、_outside_&#x200B;は、配列値自体がnullでないことを宣言します（空の配列など）。

>[!NOTE]
>
>スキーマに従ってデータを取得およびフォーマットする方法、およびそのようなロジックを特定のタイプにマッピングする方法に関するロジックは、GraphQL ランタイム実装に依存します。 ただし、実装は、ネストされたフィールドに関する理解に照らして意味のある概念的なフローに従う必要があります。ルート `Query`または`Mutation` タイプに関連付けられた解決操作が実行され、リクエストで指定された各フィールドが調べられます。 複雑な型に解決される各フィールドに対しては、すべてがスカラー値に解決されるまで、その型に対して同様の解決が行われます。

{{$include /help/_includes/graphql-rest-related-links.md}}
