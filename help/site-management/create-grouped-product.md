---
title: グループ化された製品の作成
description: REST APIとCommerce管理者を使用して、グループ化された商品を作成する方法を説明します。
kt: 14585
doc-type: video
duration: 979
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# グループ化された製品の作成

グループ化された製品は、グループとして表示されるシンプルなスタンドアロン製品で構成されます。 単一の商品のバリエーションを提供したり、季節やテーマごとにグループ化したりすることができます。 グループ化された商品を作成する前に、グループに含めるすべてのシンプルな商品がAdobe Commerceで使用可能であることを確認し、存在しないものを作成します。

このチュートリアルでは、REST APIとAdobe Commerce管理者を使用してグループ化された商品を作成する方法について説明します。

REST APIを使用して、Adminでグループ製品を作成します。

1. 空のグループ化された製品を作成します。
1. グループ化された製品で使用するシンプルな製品を作成します。
1. 空のグループ化された製品にシンプルな製品を入力します。
1. 空のグループ化された製品を作成し、単純な製品を関連付けます。

   シンプルな商品をグループ化された商品に関連付ける場合、ペイロードの並べ替え順属性（`position`）がフロントエンドで使用され、関連付けられた商品が目的の順序で表示されます。 `position`属性が指定されていない場合、グループ化された製品に追加された順序で製品が表示されます。

Adobe Commerce管理者からグループ化された商品を作成する場合は、まずシンプルな商品を作成します。 グループ化された製品を作成する準備ができたら、シンプルな製品を1つのバッチでグループ化された製品に割り当てて関連付けます。

## この動画は誰のためのものでしょうか？

* web サイトマネージャー
* コマースマーチャンダイジング
* REST APIを使用してAdobe Commerceでグループ化された商品を作成する方法を学習する新しいAdobe Commerce開発者。

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## グループ化された製品の設定

この例では、3つのシンプルな商品（最初に作成）とグループ化された商品があります。 2つの単純な製品がグループ化された製品に関連付けられ、3番目の単純な製品がグループ化された製品に追加されます。

## cURLを使用して最初のシンプルな商品を作成する

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

## cURLを使用して2番目のシンプルな製品を作成する

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

## cURLを使用して3つ目のシンプルな商品を作成する

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

## cURLを使用した空のグループ化された製品の作成

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

## 1番目と2番目のシンプルな商品をグループ化された商品に追加する

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

## 3つ目のシンプルな商品を既存のグループ化された商品に追加する

グループ化された製品に最初に関連付けられた2つの製品に使用される適切なポジション番号（`1`または`2`以外の任意の番号）を含めます。 この例では、位置は`4`です。

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

グループ化された製品から単純な製品[を](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/)削除するには、次を使用します：`DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`。

`{type}`として使用する内容を見つけるには、xdebugを使用してリクエストを取得し、$linkTypes: `related`、`crosssell`、`uupsell`、および`associated`を評価します。
![ グループ化された製品リンクの種類 – alt テキスト ](/help/assets/site-management/catalog/grouped-types.png " グループ化された製品リンクの種類がxdebug セッション中にキャプチャされました")

シンプルな製品をグループ化された製品にリンクする場合、ペイロードには次のようなセクションが含まれています。

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

ペイロードで、`link_type`値`associated`は、DELETE リクエストに必要な`{type}`値を提供します。 リクエスト URLは`/V1/products/my-new-grouped-product/links/associated/product-sku-three`に似ています。

`product-sku-three` SKUを持つグループ化された製品から`my-new-grouped-product` SKUを持つシンプルな製品を削除するcURL リクエストを参照してください。

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## cURLを使用したグループ化された製品の取得

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## 関連資料

* [ グループ化された製品の作成と管理](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
* [ グループ化された製品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html){target="_blank"}
* [Adobe Developer REST チュートリアル ](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST文書](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
