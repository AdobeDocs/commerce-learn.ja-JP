---
title: バンドル製品の作成
description: REST API とCommerce管理者を使用して、バンドル製品を作成する方法を説明します。
kt: 14589
doc-type: video
audience: all
activity: use
last-substantial-update: 2024-1-8
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 5d688e6a-ae8c-4a55-b16c-5d3ae2d1bfd5
source-git-commit: 765bf4159892416e02ea1e9b8e4fa69e396d40af
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# バンドル製品の作成

バンドル製品は、親製品の下の複数の製品をグループ化する方法です。 これらの子製品は、一連の定義済み製品とすることも、顧客に柔軟な設定オプションを提供するいくつかのバリエーションを提供することもできます。 バンドル製品タイプは設定に少し時間がかかり、設定する前に計画を立てる必要があります。 ただし、バンドル製品を提供すると、顧客が製品の選択をカスタマイズしやすくなるので、ショッピングエクスペリエンスが向上します。

例えば、`Learning to surf` という製品バンドルを web ストアで提供できます。 バンドルは、使用可能なオプションを指定する割り当てられた子製品のコンテナとして機能する親製品です。

- 標準的なサーフボード
- サーフボードの一般的なリード
- 赤いサーフボードフィン

柔軟性を高めたい場合は、子製品の複数のオプションを使用できるようにすることをお勧めします。 これには、オプションと子製品のより複雑な使用が必要です。 前の例を展開するための最終的なオプションは次のとおりです。

- 標準的なサーフボード
- サーフボードの一般的なリード
- フィンカラーの選択：
   - 赤
   - 青
   - 黄

バンドルが単純な商品の静的グループであっても、バリエーションを持つ複数の商品であっても、柔軟な設定オプションを使用すると、バンドルの商品タイプをAdobe Commerce ストア固有の強力なマーチャンダイジングツールにすることができます。

バンドル製品を作成する前に、バンドル製品に含めるすべてのシンプルな製品がAdobe Commerceで利用できることを確認します。 存在しないコンポーネントを作成します。

このチュートリアルでは、REST API とAdobe Commerce管理者を使用して、バンドル製品を作成する方法について説明します。

REST API を使用してバンドル製品を定義します。

1. バンドル製品で使用するシンプルな製品を作成します。
1. バンドル製品を作成し、シンプルな製品を関連付けます。
1. バンドル製品と、関連するすべてのオプションを取得します。
1. バンドル製品のいずれかのセクションからオプションを削除します。
1. バンドル製品のオプションを復元します。

Adobe Commerce Admin からバンドル商品を作成する場合、最初にシンプルな商品を作成するか、自動ツールを使用してウィザードを使用してシンプルな商品を作成します。

## このビデオの目的は誰ですか。

- Web サイト管理者
- e コマースマーチャンダイザー
- REST API を使用してAdobe Commerceでバンドル製品を作成する方法を学びたい、Adobe Commerceの新しい開発者向けツールです

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3426797?learn=on)

## REST を使用した製品の作成

次のコマンドは、この例ではバンドル製品の定義に必要なすべての製品を作成します。5 つのシンプルな製品と、3 つのオプションを持つ 1 つのバンドル製品です。

リクエストを送信する前に、ご使用の環境の値で例を更新してください。

- `"attribute-set": 4` を変更して、`4` を環境の属性セット ID に置き換えます。

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

次のPOSTリクエストを送信して、バンドル製品を作成します。

リクエストを送信する前に、ご使用の環境の値で例を更新してください。

- `"attribute_set_id": 4,` を変更し、`4` を環境の属性セット id に置き換えます。

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

## バンドル製品からのオプションの削除

cURL を使用して次の削除リクエストを送信することで、カタログから商品を削除せずに、バンドル商品から子商品をDELETEします。

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/bundle-products/beginner-surfboard/options/35/children/blue-fins-and-fin-plugs' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3'
```

## 製品オプションの復元

バンドル製品オプションを更新する場合は、この製品に関連付けるすべてのオプションを必ず含めてください。 元のオプションセットに 3 つの商品が含まれ、1 つが削除された場合、POSTリクエストに 3 つのオプションすべてを含めて、商品バンドルがすべてのオプションを確実に指定するようにします。 削除したオプションのみを含めた場合、更新された製品バンドルにはそのオプションのみが含まれます。

バンドル製品の作成からの応答を確認して、オプション ID を見つけます。 その応答では、`option_id` は `35` です。

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

次のPOSTリクエストを送信して、削除したオプションを追加するように製品バンドルを更新します。

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

## その他のリソース

- [ バンドル製品の作成のチュートリアル ](https://developer.adobe.com/commerce/webapi/rest/tutorials/bundle-product/){target="_blank"}
- [ バンドル製品 ](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-bundle.html){target="_blank"}
- [Adobe Developer REST チュートリアル ](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
