---
title: グループ化された製品の作成
description: REST API とコマース管理を使用してグループ化された製品を作成する方法を説明します。
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: b44376f9f30e3c02d2c43934046e86faac76f17d
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# グループ化された製品の作成

グループ化された製品は、単純なスタンドアロン製品で構成され、グループとして表示されます。 1 つの製品のバリエーションを提供したり、シーズン別またはテーマ別にグループ化したりできます。 グループ化された製品を作成する前に、グループに含めるすべてのシンプルな製品がAdobe Commerceで使用可能であることを確認し、存在しないものを作成します。

このチュートリアルでは、REST API とAdobe Commerce Admin を使用してグループ化された製品を作成する方法を学びます。

REST API を使用して、管理でグループ製品を作成します。

1. 空のグループ化された製品を作成します。
1. グループ化された製品で使用する簡単な製品を作成します。
1. 空のグループ化された製品に単純な製品を入力します。
1. 空のグループ化された製品を作成し、単純な製品を関連付けます。

   グループ化された製品に単純な製品を関連付ける場合、並べ替え順属性 (`position`) ペイロードでは、フロントエンドが関連する製品を目的の順序で表示するために使用されます。 次の場合、 `position` 属性が指定されていない場合、製品はグループ化された製品に追加された順序で表示されます。

Adobe Commerce Admin からグループ化された製品を作成する場合は、まずシンプルな製品を作成します。 グループ化されたプロダクトを作成する準備が整ったら、1 つのバッチでグループ化されたプロダクトに割り当てて、シンプルなプロダクトを関連付けます。

## このビデオは誰のためのものですか？

- Web サイトマネージャー
- e コマースマーチャンダイザー
- REST API を使用してAdobe Commerceでグループ化された製品を作成する方法を学びたい新しいAdobe Commerce開発者です。

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## グループ化された製品の設定

この例では、3 つのシンプルな製品（最初に作成）と 1 つのグループ化された製品があります。 グループ化されたプロダクトに 2 つのシンプルプロダクトが関連付けられ、グループ化されたプロダクトに 3 つ目のシンプルプロダクトが追加されます。

## cURL を使用して最初のシンプルな製品を作成

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-one",
    "name": "Simple product one",
    "attribute_set_id": 4,
    "price": 1.11,
    "type_id": "simple"
  }
}
```

## cURL を使用して 2 つ目のシンプルな製品を作成します。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-two",
    "name": "Simple Product two",
    "attribute_set_id": 4,
    "price": 2.22,
    "type_id": "simple"
  }
}
```

## cURL を使用して 3 つ目のシンプルな製品を作成します。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-three",
    "name": "Simple product three",
    "attribute_set_id": 4,
    "price": 3.33,
    "type_id": "simple"
  }
}
```

## cURL を使用して空のグループ化された製品を作成

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "product":{
        "sku":"my-new-grouped-product",
        "name":"This is my New Grouped Product",
        "attribute_set_id":4,
        "type_id":"grouped",
        "visibility":4
    }
}
'
```

## グループ化されたプロダクトに 1 つ目と 2 つ目のシンプルプロダクトを追加

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "items":[
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-one",
            "linked_product_type":"simple",
            "position":1,
            "extension_attributes":{
            "qty":1
            }
        },
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
    ]
}
'
```

## 3 つ目のシンプルな製品を既存のグループ化された製品に追加

適切な位置番号を含める ( `1` または `2`) に含まれます。これは、元々グループ化された製品に関連付けられていた最初の 2 つの製品に使用されます。 この例では、位置は `4`.

```bash
curl --location --request PUT '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2' \
--data '{
    "entity":{
        "sku":"my-new-grouped-product",
        "link_type":"associated",
        "linked_product_sku":"product-sku-three",
        "linked_product_type":"simple",
        "position":4,
        "extension_attributes":{
            "qty":1
        }
    }
}

'
```

## グループ化された製品から単純な製品を削除する

宛先 [単純な製品を削除する](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) グループ化された製品から、次を使用します。 `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`.

何をとして使用するかを見つけるには `{type}`の場合は、xdebug を使用してリクエストを取得し、$linkTypes を評価します。 `related`, `crosssell`, `uupsell`、および `associated`.
![グループ化された製品リンクタイプ — 代替テキスト](/help/assets/site-management/catalog/grouped-types.png "xdebug セッション中にキャプチャされた製品リンクタイプをグループ化しました。")

単純な製品をグループ化された製品にリンクする場合、ペイロードには次のようないくつかのセクションが含まれていました。

```bash
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
```

ペイロードでは、 `link_type` 値 `associated` には、 `{type}` の値が必要です。DELETEリクエスト。 リクエスト URL は、 `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

を使用してシンプルな製品を削除する cURL リクエストを参照してください。 `product-sku-three` グループ化された製品の SKU( `my-new-grouped-product` SKU:

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## cURL を使用したグループ化された製品の取得

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## その他のリソース

- [グループ化された製品の作成と管理](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [グループ化された製品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html){target="_blank"}
- [Adobe Developer REST チュートリアル](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
