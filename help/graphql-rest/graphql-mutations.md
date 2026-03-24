---
title: GraphQLを使用した変更の実行
description: Adobe Commerceおよび [!DNL Magento Open Source]でGraphQLを使用してミューテーションを実行する方法について説明します。 POST呼び出しを使用して最初の変更を実行します。
landing-page-description: Adobe Commerceおよび [!DNL Magento Open Source]でGraphQLを使用してミューテーションを実行する方法について説明します。 POST呼び出しを使用して最初の変更を実行します。
short-description: Adobe Commerceおよび [!DNL Magento Open Source]でGraphQLを使用してミューテーションを実行する方法について説明します。 POST呼び出しを使用して最初の変更を実行します。
kt: 13938
doc-type: video
duration: 268
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# 突然変異

これはGraphQLとAdobe Commerceのシリーズの第3部です。 ミューテーションとは、GraphQLを使用して値を保存、更新、返す機能です。


>[!VIDEO](https://video.tv.adobe.com/v/3424121?learn=on)

## このシリーズのGraphQLに関する関連動画とチュートリアル

* [第1部GraphQL – 概要](../graphql-rest/intro-graphql.md)
* [パート 2 GraphQL - クエリ](../graphql-rest/graphql-queries.md)
* [ パート 4 GraphQL - スキーマ ](../graphql-rest/graphql-schema.md)

## 突然変異の例

完全なAPI仕様では、データをクエリするだけでなく、データを作成および更新する機能を提供する必要があります。

RESTは、データを変更するリクエストと、リクエストタイプまたは「動詞」（GETとPOSTまたはPUT）を使用しないリクエストを区別します。
GraphQLを使用する場合、データを修正するクエリは、別のクエリに対応する`mutation` キーワードによって区別されます
サーバーで定義されたスキーマのルートタイプ。

ユーザーのカートに商品を追加する場合のミューテーションの例を見てみましょう。 （これには、生成されたカート IDが必要です
ログインしている顧客のセッションまたは`createEmptyCart`の突然変異を使用している場合）。

```graphql
mutation doAddToCart(
    $cartId: String!,
    $cartItems: [CartItemInput!]!
) {
    addProductsToCart(
        cartId: $cartId
        cartItems: $cartItems
    ) {
        cart {
            total_quantity
            prices {
                grand_total {
                    value
                }
            }
        }
    }
}
```

上記の突然変異が、次の変数ディクショナリとともにリクエストで送信されると想像できます。

```json
{
  "cartId": "{cart-id}",
  "cartItems": [
    {
      "quantity": 1,
      "sku": "VT01-RN-XS"
    }
  ]
}
```

最終的には、次のような反応が返ってくるかもしれません。

```json
{
  "data": {
    "addProductsToCart": {
      "cart": {
        "total_quantity": 1,
        "prices": {
          "grand_total": {
            "value": 35.2
          }
        }
      }
    }
  }
}
```

上記の例について注意すべき最も重要なことは、`mutation`の代わりに`query` キーワードを使用する以外に、
構文はクエリと同じです。 クエリと同様に、突然変異には次のものが含まれます。

* 任意の操作名（`doAddToCart`）
* 変数のリスト （例：`$cartId`）
* 括弧内に引数（例：`addProductsToCart`、値`cartId`に設定）を持つ初期フィールド （`$cartId`）
* 括弧内のフィールドの部分選択

フィールドのサブセレクションを使用すると、返すフィールドを柔軟に定義できます（
ミューテーションが完了した後に`addProductsToCart` ～ `AddProductsToCartOutput`）の値を返します。

前述のように、GraphQL スキーマで定義されたフィールドは、クエリのルートタイプ（通常は`Query`と呼ばれます）で開始されます。 同様に，
突然変異に別のルートタイプが存在します（通常は`Mutation`と呼ばれます）。 `addProductsToCart`はフィールドです
すべてのフィールドを完全に評価します。

上記の例に関するその他の注意事項：

* `!`文字のサフィックス `String`と`CartItemInput`は、変数が必要であることを示します。
* `[]`に指定された`CartItemInput` タイプの周囲の角括弧（`$cartItems`）は、リストを示します
作成することもできます。

{{$include /help/_includes/graphql-rest-related-links.md}}
