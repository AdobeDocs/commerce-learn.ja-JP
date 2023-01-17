---
title: GraphQLでのスキーマ言語
description: GraphQLに関連するスキーマについて説明します。 スキーマの説明と、興味深いパターンおよびスキーマの読み方を読みます。
landing-page-description: これはGraphQLの紹介です。 スキーマと、要素の一部の解釈方法について
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
source-git-commit: 52738be67e20cc2048bbc04afc5c01c9c5478a98
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 0%

---


# スキーマの言語

ここで扱ったクエリと変更は、サーバーで実装されている特定のデータグラフに依存します。GraphQL Runtime ではこのグラフを使用してクエリを解決します。 GraphQL仕様は、データグラフのタイプと関係を表すための非依存言語を定義します。

これまで見てきたクエリと突然変異をサポートする短縮型のスキーマを次に示します。

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

以下を詳しく調べることができます。 [GraphQLのドキュメント](https://graphql.org/learn/schema/) を参照して、ここでは示されていないいくつかの概念の構文を含む、タイプシステムの詳細を学びます。 ただし、上の例は説明が不要です。 （また、クエリ構文と構文がどの程度似ているかにも注意してください）。 GraphQLスキーマの定義は、特定の型の使用可能な引数とフィールド、およびこれらのフィールドの型を表すことの問題に過ぎません。 複合フィールドの各型には、ツリーを通じて定義が必要です。この定義は、のような単純なスカラー型になるまで続きます。 `String`.

この `input` 宣言はあらゆる点で～のようだ `type` しかし、引数の入力として使用できる型を定義します。 また、 `interface` 宣言 これは、PHP のインタフェースと同じ関数を提供します。 他のタイプは、このインターフェイスから継承されます。

構文 `[CartItemInput!]!` トリッキーに見えますが、最後にはかなり直感的です。 この `!` _inside_ 括弧は、配列内のすべての値が null 以外である必要があることを宣言し、一方、 _外部_ 配列の値自体は、null 以外（例えば、空の配列）である必要があることを宣言します。

>[!NOTE]
>
>データの取得方法と形式設定方法のロジックと、そのロジックを特定の型にマッピングする方法は、GraphQLランタイム実装次第です。 ただし、実装では、ネストされたフィールドに関する理解を踏まえて、意味を持つ概念的なフローに従う必要があります。ルートに関連付けられた解決操作 `Query` または `Mutation` タイプが実行され、リクエストで指定された各フィールドが調べられます。 複合型に解決される各フィールドに対して、同様の解決がその型に対して行われ、すべてがスカラー値に解決されるまで同様の解決が行われます。

