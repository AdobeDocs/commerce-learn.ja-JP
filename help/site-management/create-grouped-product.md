---
title: グループ化された製品の作成
description: REST API とCommerce管理者を使用して、グループ化された製品を作成する方法を説明します。
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
source-git-commit: 76a67af957b0d8c1eb64ad42f92412f338650d4b
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# グループ化された製品の作成

グループ化された製品は、グループとして表示される単純なスタンドアロン製品で構成されます。 単一の製品のバリエーションを提供したり、季節やテーマ別にグループ化したりできます。 グループ化された商品を作成する前に、グループに含めるシンプルな商品がすべてAdobe Commerceにあることを確認し、存在しない商品は作成します。

このチュートリアルでは、REST API とAdobe Commerce管理者を使用して、グループ化された商品を作成する方法を説明します。

REST API を使用して、管理者にグループ製品を作成します。

1. 空のグループ製品を作成します。
1. グループ化された製品で使用するシンプルな製品を作成します。
1. 空のグループ化製品にシンプルな製品を入力します。
1. 空のグループ製品を作成し、シンプルな製品を関連付けます。

   グループ化された製品に単純な製品を関連付けると、フロントエンドはペイロードの並べ替え順属性（`position`）を使用して、関連付けられた製品を目的の順序で表示します。 `position` 属性が指定されていない場合、製品はグループ化された製品に追加された順序で表示されます。

Adobe Commerce Admin からグループ化された商品を作成する場合は、最初にシンプルな商品を作成します。 グループ化された製品を作成する準備が整ったら、グループ化された製品にシンプルな製品を 1 つのバッチで割り当てて関連付けます。

## このビデオの目的は誰ですか。

- Web サイト管理者
- e コマースマーチャンダイザー
- Adobe Commerceの新規開発者向けに、REST API を使用してAdobe Commerceでグループ化された商品を作成する方法を説明します。

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## グループ化された製品の設定

この例では、（最初に作成された）シンプルな 3 つの製品とグループ化された製品があります。 2 つのシンプルな製品がグループ化された製品に関連付けられ、3 番目のシンプルな製品がグループ化された製品に追加されます。

## cURL を使用した最初のシンプルな製品の作成

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

## cURL を使用して 2 つ目のシンプルな製品を作成します

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

## cURL を使用した 3 つ目のシンプルな製品の作成

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

## cURL を使用して空のグループ製品を作成

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

## グループ化された製品に 1 番目と 2 番目のシンプルな製品を追加

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

## 既存のグループ化された製品に 3 番目のシンプルな製品を追加

グループ化された製品に最初に関連付けられた最初の 2 つの製品に使用される適切な位置番号（`1` または `2` を除くすべて）を含めます。 この例では、位置は `4` です。

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

## グループ化された製品からのシンプルな製品の削除

グループ化された製品から [ 単純な製品を削除 ](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) するには、`DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}` を使用します。

`{type}` として使用するものを調べるには、xdebug を使用してリクエストをキャプチャし、$linkTypes: `related`、`crosssell`、`uupsell` および `associated` を評価します。
![ グループ化された製品リンクタイプ – 代替テキスト ](/help/assets/site-management/catalog/grouped-types.png "xdebug セッション中にキャプチャされたグループ化された製品リンクタイプ ")

シンプルな製品をグループ化された製品にリンクした場合、ペイロードには次のようなセクションが含まれていました。

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

ペイロードで、`link_type` 値 `associated` はDELETEリクエストに必要な `{type}` 値を提供します。 リクエスト URL は `/V1/products/my-new-grouped-product/links/associated/product-sku-three` のようになります。

`product-sku-three` の SKU を持つグループ化された製品から `my-new-grouped-product` の SKU を持つ単純な製品を削除する cURL リクエストを参照してください。

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

- [ グループ化された製品の作成と管理 ](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [ グループ化された製品 ](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html?lang=ja){target="_blank"}
- [Adobe Developer REST チュートリアル ](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
