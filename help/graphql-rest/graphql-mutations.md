---
title: GraphQLを使用したミューテーションの実行
description: Adobe CommerceでとGraphQLを使用してミューテーションを実行する方法の概要を説明します。 [!DNL Magento Open Source]. POST呼び出しを使用して最初のミューテーションを実行します。
landing-page-description: Adobe CommerceでとGraphQLを使用してミューテーションを実行する方法の概要を説明します。 [!DNL Magento Open Source]. POST呼び出しを使用して最初のミューテーションを実行します。
short-description: Adobe CommerceでとGraphQLを使用してミューテーションを実行する方法の概要を説明します。 [!DNL Magento Open Source]. POST呼び出しを使用して最初のミューテーションを実行します。
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


>[!VIDEO](https://video.tv.adobe.com/v/3424121?learn=on)

## このシリーズのGraphQLに関する関連ビデオとチュートリアル

* [第 1 部GraphQL – はじめに](../graphql-rest/intro-graphql.md)
* [第 2 部GraphQL - クエリ](../graphql-rest/graphql-queries.md)
* [第 4 部GraphQL - スキーマ](../graphql-rest/graphql-schema.md)

## 突然変異の例

完全な API 仕様では、データのクエリ機能だけでなく、データの作成と更新機能も提供する必要があります。

REST は、データを変更するリクエストと、リクエストタイプまたは「動詞」（GETとPOSTまたはPUT）を使用しないリクエストを区別します。
GraphQLを使用する場合、データを変更するクエリは以下によって区別されます。 `mutation` サーバーで定義されているスキーマ内の別のルートタイプに対応するキーワード。

ユーザーの買い物かごに製品を追加するための、このサンプルのミューテーションを見てみましょう。 （これには、ログインしている顧客のセッションまたはを使用して生成された買い物かご ID が必要です） `createEmptyCart` 変異）

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

上記の例について注意すべき主な点は、 `mutation` キーワードの代わり `query`の場合、構文はクエリと同じです。 クエリと同様に、ミューテーションには次が含まれます。

* 任意の操作名（`doAddToCart`）
* 変数のリスト（例： `$cartId`）
* 最初のフィールド （`addProductsToCart`）に設定し、引数（例： `cartId`、の値に設定します。 `$cartId`）を括弧で囲みます
* 中括弧で囲まれたフィールドの部分選択

フィールドを副選択すると、返すフィールドを（の戻り値として割り当てられたタイプから）柔軟に定義できます `addProductsToCart` - `AddProductsToCartOutput`）を選択します。

前述のように、GraphQL スキーマで定義されたフィールドは、クエリのルートタイプ（通常は `Query`）に設定します。 同様に、突然変異に別のルートタイプが存在します（一般的にはと呼ばれます） `Mutation`）に設定します。 `addProductsToCart` は、そのルートタイプ上のフィールドです。

上記の例に関するその他の注意事項を次に示します。

* この `!` 文字のサフィックス `String` および `CartItemInput` は、変数が必須であることを示します。
* 角括弧（`[]`）に設定します `CartItemInput` に指定されたタイプ `$cartItems` 単一の値ではなく、そのタイプのリストを示します。

{{$include /help/_includes/graphql-rest-related-links.md}}
