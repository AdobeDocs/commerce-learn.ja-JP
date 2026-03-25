---
title: 設定可能な製品の作成
description: REST APIとCommerce管理者を使用して、設定可能な製品を作成する方法を説明します。
kt: 14586
doc-type: video
duration: 1760
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 112bec9a-0f8e-4252-8c52-f486a5e663b5
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---

# 設定可能な製品の作成

コンフィグ商品は、複数のシンプルな商品の親商品です。 購入者が特定の製品バリエーションを選択するために、1つまたは複数の選択を行うことを要求するように、設定可能な製品を定義します。 例えば、商品がシャツの場合、買い手はサイズと色のオプションを選択してシャツを選択する必要があります。

設定可能な製品では、より多くのSKUを使用し、最初は設定に少し時間がかかるかもしれませんが、最終的には時間を節約することができます。 ビジネスを拡大したい場合は、複数のオプションがある商品に最適な選択肢である、設定可能な商品タイプを選択しましょう。

設定可能な製品を作成する前に、設定可能な製品に含めるすべてのシンプルな製品がAdobe Commerceで使用可能であることを確認します。 存在しないものを作成します。

このチュートリアルでは、REST APIとAdobe Commerce管理者を使用して設定可能な製品を作成する方法を説明します。

REST APIを使用して、設定可能な製品を作成します。

1. 後続のAPI呼び出しにID番号を使用するために、[属性セット &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html?lang=ja)の属性を取得します。
1. 設定可能な製品で使用するシンプルな製品を作成します。
1. 空の設定可能な製品を作成し、シンプルな製品を関連付けます。
1. 設定可能な製品の製品属性を設定します。
1. 空の設定可能な製品にシンプルな製品を入力します。
1. 設定可能な製品とすべての属性を取得します。
1. 設定可能な製品に割り当てられた子の製品を取得します。
1. シンプルな製品と設定可能な製品の関連付けを削除します。

Adobe Commerce管理者から設定可能な商品を作成する場合は、最初にシンプルな商品を作成するか、ウィザードを使用して新しいシンプルな商品を作成する自動ツールを使用できます。

## この動画は誰のためのものでしょうか？

* web サイトマネージャー
* コマースマーチャンダイジング
* REST APIを使用してAdobe Commerceで設定可能な製品を作成する方法を学びたい新しいAdobe Commerce開発者

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## cURLを使用したカラー属性の取得

この例では、すべての個々の属性を含む属性セット全体が、属性セット 10に対して返されます。 長くなる可能性があり、何百行もの行が珍しくありません。 応答を確認する場合、色の属性IDは中央になる可能性があります。 grepやその他の方法を使用して結果を検索し、これらの値の検索を迅速化します。 私の応答は665行付近で、JSON応答から次のスニペットに含まれています。

```json
...
{
        "attribute_id": 93,
        "attribute_code": "color",
        "frontend_input": "select",
        "entity_type_id": "4",
        "is_required": false,
        "options": [
            {
                "label": " ",
                "value": ""
            },
            {
                "label": "Red",
                "value": "13"
            },
            {
                "label": "Blue",
                "value": "14"
            },
            {
                "label": "Green",
                "value": "15"
            }
        ],
...
```


設定可能な製品を設定するための属性IDを取得するには、次のcURL リクエストの`attribute-sets/10/attributes`部分を更新して、`10`を環境内の属性セット IDに置き換えます。 このリクエストはGET メソッドを使用します。

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## cURLを使用して最初のシンプルな商品を作成する

### 環境IDと製品の詳細の調整

APIを使用して最初のシンプルな製品を作成し、cURLを使用して次のPOST リクエストを送信します。

リクエストを送信する前に、環境の値で例を更新します。

* `"attribute-set": 10`を変更して、`10`を環境の属性セット IDに置き換えます。
* `"value": "13"`を変更して、`13`を環境の値に置き換えます。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-red",
    "name": "Kids Hawaiian Ukulele Red",
    "attribute_set_id": 10,
    "price": 12.50,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "10",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "13"
        }
    ]
  }
}
'
```

## cURLを使用して2番目のシンプルな製品を作成する

APIを使用して2番目のシンプルな製品を作成し、cURLを使用して次のPOST リクエストを送信します。

リクエストを送信する前に、環境の値で例を更新します。

* `"attribute_set_id": 10,`を変更し、`10`を環境内の属性セット IDに置き換えます。
* `"value": "14"`を変更し、`14`を環境の値に置き換えます。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Blue",
    "name": "Kids Hawaiian Ukulele Blue",
    "attribute_set_id": 10,
    "price": 15,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "20",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "14"
        }
    ]
  }
}
'
```

## cURLを使用して3つ目のシンプルな商品を作成する

cURLを使用して次のPOST リクエストを送信し、3番目のシンプルな製品を作成します。

リクエストを送信する前に、環境の値で例を更新します。

* `"attribute_set_id": 10,`を変更して、`10`を環境の属性セット IDに置き換えます。
* `"value": "15"`を変更し、`15`を環境の値に置き換えます。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Green",
    "name": "Kids Hawaiian Ukulele Green",
    "attribute_set_id": 10,
    "price": 25,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "30",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "15"
        }
    ]
  }
}
'
```

## cURLを使用した空の設定可能な製品の作成

cURLを使用して次のPOST リクエストを送信し、空の設定可能な製品を作成します。

リクエストを送信する前に、環境の値で例を更新します。

* `"attribute_set_id": 10,`を変更し、`10`を環境の属性セット IDに置き換えます。
* `"value": "93"`を変更し、`93`を環境の値に置き換えます。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele",
    "name": "Kids Hawaiian Ukulele",
    "attribute_set_id": 10,
    "status": 1,
    "visibility": 4,
    "type_id": "configurable",
    "weight": "0.5",
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "93"
        }
    ]
  }
}'
```

## 設定可能な製品で使用できるオプションを設定します

cURLを使用して次のPOST リクエストを送信することで、設定可能な製品で使用可能なオプションを設定します。

リクエストを送信する前に、`"attribute_id": 93,`を変更して、`93`を環境の属性IDに置き換えます。

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/options' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "option": {
    "attribute_id": "93",
    "label": "Color",
    "position": 0,
    "is_use_default": true,
    "values": [
      {
        "value_index": 9
      }
    ]
  }
}'
```

設定可能な製品（親）のオプションを設定するのを忘れた場合、子製品を設定可能な製品に関連付けようとするとエラーが発生します。 エラーメッセージは、次の例に似ています。

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## 子製品を設定可能な

これで、3つのシンプルな製品を作成しました。

* `"Kids Hawaiian Ukulele Red"`,
* `"Kids-Hawaiian-Ukulele-Blue"`
* `"Kids-Hawaiian-Ukulele-Green"`

以下のPOST リクエストを送信して、これらのシンプルな製品を設定可能な製品の子として追加します。 製品ごとに個別のリクエストを送信します。

リクエストごとに、`childSKU`値を、追加する子製品の値に更新します。 次の例では、SKU `kids-Hawaiian-Ukulele-red`を持つ設定可能な製品に単純な製品`Kids-Hawaiian-Ukulele-red`を割り当てます。


```bash
curl --location '{{your.url.here}}rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/child' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "childSku": "Kids-Hawaiian-Ukulele-red"
}
'
```

## cURLを使用した設定可能な製品の取得

これで、3つの子SKUが割り当てられた設定可能な製品を作成しました。 cURLを使用して次のGET リクエストを送信すると、割り当てられた商品のリンク IDを確認できます。 このリクエストは、設定可能な製品に関する詳細情報を返します。

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

次の例では、GET メソッドを使用しています

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 設定可能な製品に関連付けられている子の製品を取得する

次のGET リクエストを送信して、設定可能な製品に関連付けられている子のみを返します。 応答には、SKUと価格を含む子製品のすべての属性が含まれます。

次の例では、GET メソッドを使用しています

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 子製品を親から削除または削除する

次のDELETE リクエストをcURLを使用して送信すると、カタログから商品を削除せずに、設定可能な商品から子商品を削除できます。

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 関連資料

* [設定可能な製品チュートリアルを作成](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
* [構成可能な製品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html?lang=ja){target="_blank"}
* [Adobe Developer REST チュートリアル &#x200B;](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST文書](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
