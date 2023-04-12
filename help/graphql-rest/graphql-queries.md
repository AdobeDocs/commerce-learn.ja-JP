---
title: GraphQLを使用したクエリの実行
description: Adobe CommerceでGraphQLを使用してクエリを実行する方法と [!DNL Magento Open Source]. これは、GET呼び出しとPOST呼び出しを使用するGraphQLの紹介です。
landing-page-description: Adobe CommerceでGraphQLを使用してクエリを実行する方法と [!DNL Magento Open Source]. これは、GET呼び出しとPOST呼び出しを使用するGraphQLの紹介です。
short-description: Adobe CommerceでGraphQLを使用してクエリを実行する方法と [!DNL Magento Open Source]. これは、GET呼び出しとPOST呼び出しを使用するGraphQLの紹介です。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---

# GraphQLクエリ

本格的な例を使用して、GraphQLクエリ構文を簡単に説明します。 (https://venia.magento.com/graphqlに対しては、ご自身で試してみてください。)

次のGraphQLクエリを 1 つずつ分類して確認します。

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

上記のクエリに対するGraphQLサーバーからの妥当な応答は、次のようになります。

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

上記の例では、サーバーで定義されているAdobe Commerce用の標準のGraphQLスキーマを使用しています。 このリクエストでは、複数のタイプのデータを一度にクエリします。 クエリは必要なフィールドを正確に表し、返されるデータはクエリ自体と同様に形式設定されます。

>[!NOTE]
>
>GraphQLクライアントは、実際に送信される HTTP リクエストの形式を難読化しますが、これを見つけるのは簡単です。 ブラウザーベースのクライアントを使用している場合は、 [!UICONTROL Network] タブに表示されます。 リクエストに、「クエリ」で構成される生の本文が含まれていることがわかります。 `{string}`&quot;、ここで `{string}` は、単にクエリ全体の生の文字列です。 リクエストがGETとして送信される場合、クエリは、代わりにクエリ文字列パラメーター「query」でエンコードされます。 REST とは異なり、HTTP リクエストタイプは重要ではなく、クエリの内容だけが重要です。


## 必要な項目のクエリ

`country` および `categories` この例では、2 種類のデータに対して 2 種類の「クエリ」を表しています。 REST のような従来の API パラダイムとは異なり、各データ型に対して個別で明示的なエンドポイントを定義します。 GraphQLでは、多数のタイプのデータを一度に取得できる式を使用して、単一のエンドポイントに対してクエリを実行する柔軟性を提供します。

同様に、クエリは、両方に必要なフィールドを正確に指定します `country` (`id` および `full_name_english`) および `categories` (`items`を返します。これ自体にはフィールドのサブセレクションが含まれています )。また、返されるデータは、そのフィールドの指定を反映しています。 これらのデータタイプには、おそらく他にも多くのフィールドが使用できますが、要求したもののみが返されます。


>[!NOTE]
>
>次のような戻り値が返されるのに気付くかもしれません： `items` は、実際には _配列_ 値の値が含まれていても、そのサブフィールドを直接選択することになります。 フィールドのタイプがリストの場合、GraphQLはリスト内の各項目に適用する下位選択を暗黙的に認識します。

## 引数

返されるフィールドは各型の中括弧内で指定されますが、名前付き引数と値は型名の後の括弧内で指定されます。 引数はオプションで、多くの場合、クエリ結果のフィルター適用、書式設定、変換の方法に影響を与えます。

次を渡します： `id` ～に対する議論 `country`クエリする特定の国を指定し、 `filters` ～に対する引数 `categories`.

## ずっと下のフィールド

～を考える傾向があるが `country` および `categories` 個別のクエリまたはエンティティとして、クエリで表されるツリー全体は、実際にはフィールド以外の何もで構成されません。 次の式： `products` の構文との違いはない `categories`. どちらも畑で、建築に違いはありません。

任意のGraphQLデータグラフには単一の「ルート」タイプがあります ( 通常、 `Query`) をクリックしてツリーを開始し、エンティティと見なされる可能性の高いタイプが、このルートのフィールドに割り当てられます。 この例のクエリでは、実際にはルートタイプに対して 1 つの汎用クエリを作成し、フィールドを選択します `country` および `categories`. その後、これらのフィールドのサブフィールドを選択します。また、複数レベルの深さがある可能性もあります。 フィールドの戻り値の型が複雑な型（例えば、スカラー型ではなく独自のフィールドを持つ型）の場合は、引き続きフィールドを選択します。

ネストされたフィールドのこの概念は、 `products` (`pageSize` および `currentPage`) を使用します。 `categories` フィールドに入力します。

![GraphQL Field Tree](../assets/graphql-field-tree.png)

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

最初に注意すべきことは、キーワードを追加することです `query` クエリの開始括弧の前に、操作名 (`getProducts`) をクリックします。 この操作名は任意です。サーバースキーマ内の要素には対応しません。 この構文は、変数の導入をサポートするために追加されました。

前のクエリでは、フィールドの引数の値を文字列または整数として直接ハードコードしました。 ただし、GraphQLの仕様では、変数を使用してユーザー入力をメインクエリと分離するファーストクラスのサポートが用意されています。

新しいクエリでは、クエリ全体の開始括弧の前の括弧を使用して、 `$search` 変数（変数では常にドル記号のプレフィックス構文を使用します）。 これは、 `search` ～に対する引数 `products`.

クエリに変数が含まれる場合、GraphQLリクエストでは、値の個別の JSON エンコード済み辞書をクエリ自体に含める必要があります。 上記のクエリでは、クエリ本文に加えて、次の変数値の JSON を送信できます。

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>独自のAdobe Commerceインスタンスではなく、Venia の例のサイトに対してこれらのクエリを試みる場合、返される結果は空になる可能性が高いです。 `related_products`.

テストに使用しているGraphQL対応クライアント（Altair や GraphiQL など）では、UI はクエリとは別に変数 JSON を入力することをサポートします。

GraphQLクエリの実際の HTTP リクエストに「query: `{string}`」と呼ばれる変数ディクショナリを含むリクエストには、単に追加の「変数」が含まれます。 `{json}`」 `{json}` は、変数値を含む JSON 文字列です。

新しいクエリでは、 _フラグメント_ (`productDetails`) を使用して、同じフィールド選択を複数の場所で再利用できます。 [フラグメントの詳細を表示](https://graphql.org/learn/queries/#fragments){target="_blank"} (GraphQLドキュメント ) を参照してください。

{{$include /help/_includes/graphql-rest-related-links.md}}
