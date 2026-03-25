---
title: バンドル製品の作成
description: REST APIとCommerce Adminを使用してバンドル製品を作成する方法を説明します。
kt: 14589
doc-type: video
duration: 1335
audience: all
activity: use
last-substantial-update: 2024-1-8
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 5d688e6a-ae8c-4a55-b16c-5d3ae2d1bfd5
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# バンドル製品の作成

バンドル製品とは、複数の製品を親製品の下にグループ化する方法です。 これらの子製品は、定義済みの製品セットであったり、顧客に柔軟な設定オプションを提供するバリエーションをいくつか提供したりすることができます。 バンドル製品タイプの設定には少し時間がかかるため、設定する前に計画を立てる必要があります。 ただし、バンドル商品を提供することで、顧客が商品の選択を簡単にカスタマイズできるようにし、ショッピング体験を向上させることができます。

例えば、web ストアで`Learning to surf`という製品バンドルを提供できます。 バンドルは、使用可能なオプションを指定する、割り当てられた子製品のコンテナとして機能する親製品です。

* 標準的なサーフボード
* 典型的なサーフボードのリード
* 赤いサーフボードフィン

追加の柔軟性が必要な場合は、子製品のいくつかのオプションを許可することをお勧めします。 これには、オプションと子製品のより複雑な使用が必要です。 前の例を拡張するために、最終的なオプションは次のとおりです。

* 標準的なサーフボード
* 典型的なサーフボードのリード
* フィンカラーの選択：
   * 赤
   * 青
   * 黄色

バンドルがシンプルな商品の静的グループであろうと、バリエーションの複数の商品であろうと、柔軟な設定オプションにより、バンドル商品タイプは、Adobe Commerceストアにとって一意の強力なマーチャンダイジングツールになります。

バンドル製品を作成する前に、バンドル製品に含めるすべてのシンプルな製品がAdobe Commerceで使用可能であることを確認します。 存在しないものを作成します。

このチュートリアルでは、REST APIとAdobe Commerce管理者を使用してバンドル製品を作成する方法を説明します。

REST APIを使用して、バンドル製品を定義します。

1. バンドル製品で使用するシンプルな製品を作成します。
1. バンドル製品を作成し、単純な製品を関連付けます。
1. バンドル製品と関連するすべてのオプションを取得します。
1. バンドル製品の1つのセクションからオプションを削除します。
1. バンドル製品のオプションを復元します。

Adobe Commerce Adminからバンドル製品を作成する場合は、まずシンプルな製品を作成するか、自動ツールを使用してウィザードを使用してシンプルな製品を作成できます。

## この動画は誰のためのものでしょうか？

* web サイトマネージャー
* コマースマーチャンダイジング
* REST APIを使用してAdobe Commerceでバンドル製品を作成する方法を学びたい新しいAdobe Commerce開発者

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3426797?learn=on)

## RESTを使用した製品の作成

次のコマンドは、この例でバンドル製品を定義するために必要なすべての製品を作成します。5つの単純な製品と、3つのオプションを持つ1つのバンドル製品です。

リクエストを送信する前に、環境の値で例を更新します。

* `"attribute-set": 4`を変更して、`4`を環境の属性セット IDに置き換えます。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "10-ft-beginner-surfboard",
    "name": "10 Foot Beginner surfboard",
    "attribute_set_id": 4,
    "price": 100,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "8-ft-surboard-leash",
    "name": "8 Foot surboard leash",
    "attribute_set_id": 4,
    "price": 50,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "red-fins-and-fin-plugs",
    "name": "Red fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "blue-fins-and-fin-plugs",
    "name": "Blue fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "yellow-fins-and-fin-plugs",
    "name": "Yellow fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

## バンドル製品を作成し、シンプルな製品をオプションとして割り当てます

次のPOST リクエストを送信して、バンドル製品を作成します。

リクエストを送信する前に、環境の値で例を更新します。

* `"attribute_set_id": 4,`を変更し、`4`を環境の属性セット IDに置き換えます。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "beginner-surfboard",
    "name": "Beginner Surfboard",
    "attribute_set_id": 4,
    "status": 1,
    "visibility": 4,
    "type_id": "bundle",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      },
      "bundle_product_options": [
        {
          "option_id": 0,
          "position": 1,
          "sku": "surfboard-essentials",
          "title": "Beginners 10ft Surfboard",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "10-ft-beginner-surfboard",
              "option_id": 1,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 100,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 1,
          "position": 2,
          "sku": "surboard-leash",
          "title": "Surfboard leash",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "8-ft-surboard-leash",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 50,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 3,
          "position": 2,
          "sku": "surfboard-color",
          "title": "Choose a color for the fins and fin plugs",
          "type": "radio",
          "required": true,
          "product_links": [
            {
              "sku": "red-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "blue-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 2,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "yellow-fins-and-fin-plugs",
              "option_id": 3,
              "qty": 1,
              "position": 3,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        }
      ]
    },
    "custom_attributes": [
      {
        "attribute_code": "price_view",
        "value": "0"
      }
    ]
  },
  "saveOptions": true
}
'
```

## バンドル製品からオプションを削除する

次のDELETE リクエストをcURLを使用して送信し、カタログから商品を削除せずに、バンドル商品から子商品を削除します。

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/bundle-products/beginner-surfboard/options/35/children/blue-fins-and-fin-plugs' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3'
```

## 製品オプションの復元

バンドル製品のオプションを更新する場合は、この製品に関連付けるすべてのオプションを必ず含めてください。 元のオプションのセットに3つの製品が含まれており、1つが削除された場合、POST リクエストに3つのオプションをすべて含めて、製品バンドルがすべてのオプションを指定するようにします。 削除したオプションのみを含めた場合、更新された製品バンドルには、そのオプションのみが含まれます。

バンドル製品の作成からの応答を確認して、オプション IDを探します。 この応答では、`option_id`は`35`です。

```json
...
,
            {
                "option_id": 35,
                "title": "added back color options for fins and fin plugs",
                "required": true,
                "type": "radio",
                "position": 2,
                "sku": "beginner-surfboard",
                "product_links": [
                    {
                        "id": "77",
                        "sku": "red-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 1,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "78",
                        "sku": "blue-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 2,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "79",
                        "sku": "yellow-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 3,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    }
                ]
            }
...
```

次のPOST リクエストを送信して、削除したオプションを追加するように製品バンドルを更新します。

```bash
curl --location '{{your.url.here}}/rest/default/V1/bundle-products/options/add' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "option": {
    "option_id": 35,
    "title": "Choose a color for the fins and fin plugs",
    "required": true,
    "type": "radio",
    "position": 2,
    "sku": "beginner-surfboard",
    "product_links": [
      {
        "sku": "red-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 1,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "blue-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 2,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "yellow-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 3,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      }
    ],
    "extension_attributes": {}
  }
}'
```

## 関連資料

* [&#x200B; バンドル製品チュートリアルの作成](https://developer.adobe.com/commerce/webapi/rest/tutorials/bundle-product/){target="_blank"}
* [&#x200B; バンドル製品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-bundle.html?lang=ja){target="_blank"}
* [Adobe Developer REST チュートリアル &#x200B;](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST文書](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
