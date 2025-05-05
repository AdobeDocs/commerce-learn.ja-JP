---
title: GraphQLを使用したミューテーションの実行
description: Adobe CommerceでGraphQLを使用してミューテーションを実行する方法の概要を説明します。 [!DNL Magento Open Source] POST呼び出しを使用して最初のミューテーションを実行します。
landing-page-description: Adobe CommerceでGraphQLを使用してミューテーションを実行する方法の概要を説明します。 [!DNL Magento Open Source] POST呼び出しを使用して最初のミューテーションを実行します。
short-description: Adobe CommerceでGraphQLを使用してミューテーションを実行する方法の概要を説明します。 [!DNL Magento Open Source] POST呼び出しを使用して最初のミューテーションを実行します。
kt: 13938
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# 突然変異

これは、GraphQLとAdobe Commerceのシリーズの第 3 部です。 ミューテーションとは、GraphQLを使用して値を保存、更新および返す機能です。


>[!VIDEO](https://video.tv.adobe.com/v/3441922?learn=on&captions=jpn)

## このシリーズのGraphQLに関する関連ビデオとチュートリアル

* [第 1 部GraphQL – はじめに](../graphql-rest/intro-graphql.md)
* [第 2 部GraphQL - クエリ](../graphql-rest/graphql-queries.md)
* [ 第 4 部GraphQL - スキーマ ](../graphql-rest/graphql-schema.md)

## 突然変異の例

完全な API 仕様では、データのクエリ機能だけでなく、データの作成と更新機能も提供する必要があります。

REST は、データを変更するリクエストと、リクエストタイプまたは「動詞」（GETとPOSTまたはPUT）を使用しないリクエストを区別します。
GraphQLを使用する場合、データ変更クエリは、別のに対応する `mutation` キーワードによって区別されます
サーバーで定義されたスキーマのルートタイプ。

ユーザーの買い物かごに製品を追加するための、このサンプルのミューテーションを見てみましょう。 （これには、生成されたカート ID が必要です。
ログインしている顧客のセッション用、または `createEmptyCart` ミューテーションを使用する場合）

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

上記のミューテーションが次の変数ディクショナリと共にリクエストで送信されると想像できます。

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

最後に、次のような応答が返されます。

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

上記の例について注意しておくべきことは、`query` の代わりに `mutation` キーワードを使用する以外に、
構文はクエリと同じです。 クエリと同様に、ミューテーションには次が含まれます。

* 任意の操作名（`doAddToCart`）
* 変数のリスト（例：`$cartId`）
* 括弧内に引数（例：`cartId` は `$cartId` の値に設定）を含む初期フィールド（`addProductsToCart`）
* 中括弧で囲まれたフィールドの部分選択

フィールドの副選択を使用すると、返されるフィールド（として割り当てられたタイプから）を柔軟に定義できます。
ミューテーションが完了した後の戻り値 `addProductsToCart`～`AddProductsToCartOutput`）。

前述のように、GraphQL スキーマで定義されたフィールドは、クエリのルートタイプ（通常 `Query` と呼ばれます）で開始されます。 同様に、
突然変異（一般的に `Mutation` と呼ばれる）には、別のルートタイプが存在します。 `addProductsToCart` is a field
そのルートタイプで。

上記の例に関するその他の注意事項を次に示します。

* `String` と `CartItemInput` の後ろに `!` 文字が付いている場合は、変数が必須であることを示しています。
* リストを示すために指定された `CartItemInput` タイプの前後の角かっこ（`[]`） `$cartItems`
（単一の値ではなく、そのタイプの）

{{$include /help/_includes/graphql-rest-related-links.md}}
