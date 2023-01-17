---
title: GraphQLを使用したミューテーションの実行
description: Adobe CommerceでのGraphQLを使用した突然変異の実行と、 [!DNL Magento Open Source]. ミューテーションコールを使用して、最初のPOSTを実行します。
landing-page-description: Adobe CommerceでのGraphQLを使用した突然変異の実行と、 [!DNL Magento Open Source]. ミューテーションコールを使用して、最初のPOSTを実行します。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
source-git-commit: 52738be67e20cc2048bbc04afc5c01c9c5478a98
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# 突然変異

完全な API 仕様には、データをクエリするだけでなく、データを作成および更新する機能を提供する必要があります。

REST は、GETを変更するリクエストと、リクエストの種類または「動詞」( データとPOSTまたはPUT) を使用しないリクエストを区別します。
GraphQLを使用する場合、データ変更クエリは `mutation` サーバーで定義されたスキーマ内の別のルートタイプに対応するキーワード。

ユーザーの買い物かごに製品を追加する際の、このミューテーションの例を見てみましょう。 ( これには、ログインした顧客のセッション用に生成された買い物かご ID が必要です。 `createEmptyCart` 突然変異 )

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

上記のミューテーションが、次の変数ディクショナリと共にリクエストで送信されると想像できます。

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

上記の例に関して注意すべき主な点は、 `mutation` キーワードの代わりに `query`の場合の構文はクエリと同じです。 クエリと同様に、ミューテーションには次のものが含まれます。

* 任意の操作名 (`doAddToCart`)
* 変数のリスト ( 例： `$cartId`)
* 最初のフィールド (`addProductsToCart`) を引数 ( 例： `cartId`、の値に設定 `$cartId`) を括弧内に
* 中括弧で囲まれたフィールドの下位選択

「フィールド」サブ選択では、返すフィールドを柔軟に定義できます ( `addProductsToCart` - `AddProductsToCartOutput`) と同じ値を持つ ) に変異が完了した後で呼び出される問題を修正しました。

前述のように、GraphQLスキーマに定義されたフィールドは、クエリのルートタイプで開始します ( 通常、 `Query`) をクリックします。 同様に、突然変異に対して別のルートタイプが存在する ( 通常は `Mutation`) をクリックします。 `addProductsToCart` は、そのルートタイプのフィールドです。

上記の例に関するその他の注意事項を次に示します。

* この `!` 文字サフィックス `String` および `CartItemInput` は、変数が必須であることを示します。
* 角括弧 (`[]`) を `CartItemInput` 指定されたタイプ `$cartItems` 単一の値ではなく、そのタイプのリストを示します。