---
title: 設定可能な製品の作成
description: REST API とコマース管理を使用して設定可能な製品を作成する方法を説明します。
kt: 14586
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: 76716d4c9530963f198a855e101c76b6374c6d75
workflow-type: tm+mt
source-wordcount: '979'
ht-degree: 0%

---


# 設定可能な製品の作成

設定可能な製品は、複数のシンプルな製品の親製品です。 設定可能な製品を定義して、購入者が特定の製品バリエーションを選択するための 1 つ以上の選択を行う必要があります。 例えば、製品がシャツの場合、購入者はサイズと色のオプションを選択してシャツを選択する必要があります。

設定可能な製品では、より多くの SKU を使用し、最初は設定に少し時間がかかる場合がありますが、最終的に時間を節約できます。 ビジネスを拡大する予定がある場合は、複数のオプションを持つ製品に適した製品タイプを選択できます。

設定可能な製品を作成する前に、設定可能な製品に含めるすべてのシンプルな製品がAdobe Commerceで使用できることを確認します。 存在しないを作成します。

このチュートリアルでは、REST API とAdobe Commerce Admin を使用して設定可能な製品を作成する方法を説明します。

REST API を使用して、設定可能な製品を作成します。

1. の属性を取得する [属性セット](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html) を使用して、後続の API 呼び出しに ID 番号を使用します。
1. 設定可能な製品で使用するシンプルな製品を作成します。
1. 空の設定可能な製品を作成し、単純な製品を関連付けます。
1. 設定可能な製品の製品属性を設定します。
1. 空の設定可能な製品に単純な製品を入力します。
1. 設定可能な製品とすべての属性を取得します。
1. 設定可能な製品に割り当てられた子製品を取得します。
1. 設定可能な製品に対するシンプルな製品の関連付けを削除します。

Adobe Commerce Admin から設定可能な製品を作成する場合は、最初にシンプルな製品を作成するか、ウィザードを使用して新しいシンプルな製品を作成する自動ツールを使用できます。

## このビデオは誰のためのものですか？

- Web サイトマネージャー
- e コマースマーチャンダイザー
- REST API を使用してAdobe Commerceで設定可能な製品を作成する方法を学びたい新しいAdobe Commerce開発者

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## cURL を使用したカラー属性の取得

この例では、属性セット 10 に対して、個々の属性をすべて含む属性セット全体が返されます。 長い場合もあり、数百の行が珍しくありません。 応答を確認する際、色の属性 ID が途中にある可能性があります。 grep または他の方法を使用して結果を検索し、これらの値の検索を効率化します。 私の応答は 665 行目に近く、JSON 応答の次のスニペットに含まれています。

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


設定可能な製品を設定するための属性 ID を取得するには、 `attribute-sets/10/attributes` 次の cURL リクエストの一部を `10` を使用して、環境の属性セット ID を設定します。 このリクエストでは、GETメソッドを使用します。

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## cURL を使用して最初のシンプルな製品を作成

### 環境 ID と製品の詳細の調整

API を使用して最初のシンプルな製品を作成し、cURL を使用して次のPOSTリクエストを送信します。

リクエストを送信する前に、使用している環境の値で例を更新します。

- 変更 `"attribute-set": 10` 置き換える `10` 環境の属性セット ID を持つ。
- 変更 `"value": "13"` 置き換える `13` を、環境の値で置き換えます。

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

## cURL を使用して 2 つ目のシンプルな製品を作成します。

API を使用して 2 つ目のシンプルな製品を作成し、cURL を使用して次のPOSTリクエストを送信します。

リクエストを送信する前に、使用している環境の値で例を更新します。

- 変更 `"attribute_set_id": 10,` および置換 `10` と、環境のからの属性セット id を組み合わせます。
- 変更 `"value": "14"` および置換 `14` を、環境の値で置き換えます。


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

## cURL を使用して 3 つ目のシンプルな製品を作成します。

API を使用して 3 つ目のシンプルな製品を作成し、cURL を使用して次のPOSTリクエストを送信します。

リクエストを送信する前に、使用している環境の値で例を更新します。

- 変更 `"attribute_set_id": 10,` 置き換える `10` 環境の属性セット ID を持つ。
- 変更 `"value": "15"` および置換 `15` を、環境の値で置き換えます。

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

## cURL を使用して空の設定可能な製品を作成

API を使用して空の設定可能な製品を作成し、cURL を使用して次のPOSTリクエストを送信します。

リクエストを送信する前に、使用している環境の値で例を更新します。

- 変更 `"attribute_set_id": 10,` および置換 `10` 環境の属性セット id を使用します。
- 変更 `"value": "93"` および置換 `93` を、環境の値で置き換えます。

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

## 設定可能な製品で使用可能なオプションを設定

API を使用して、cURL を使用して次のPOSTリクエストを送信することで、設定可能な製品で使用可能なオプションを設定します。

リクエストを送信する前に、 `"attribute_id": 93,` 置き換える `93` 環境の属性 id を持つ。

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

設定可能な製品（親）のオプションを設定し忘れた場合、子製品を設定可能な製品に関連付けようとするとエラーが発生します。 次の例のようなエラーメッセージが表示されます。

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## 子製品を設定可能な製品にリンク

次に、3 つのシンプルな製品を作成しました。

- `"Kids Hawaiian Ukulele Red"`,
- `"Kids-Hawaiian-Ukulele-Blue"`
- `"Kids-Hawaiian-Ukulele-Green"`

API を使用して、これらのシンプルな製品を設定可能な製品の子として追加し、各製品に対して次のPOSTリクエストを送信します。 各製品に対して個別のリクエストを送信します。

リクエストごとに、 `childSKU` 値と、追加する子製品の値。 次の例では、単純な製品を割り当てます。 `kids-Hawaiian-Ukulele-red` を SKU 付きの設定可能な製品に追加 `Kids-Hawaiian-Ukulele-red`.


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

## cURL を使用した設定可能な製品の取得

これで、3 つの子 SKU が割り当てられた設定可能な製品が作成されました。 cURL を使用して以下のGETリクエストを送信するために、API によって割り当てられた製品のリンクされた ID を確認できます。 このリクエストは、設定可能な製品に関する詳細情報を返します。

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

次の例では、GETメソッドを使用します

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 設定可能な製品に関連付けられた子製品を取得する

このリクエストは、設定可能な製品に関連付けられている子のみを返します。 この応答には、SKU や価格を含む子製品のすべての属性が含まれます。

次の例では、GETメソッドを使用します

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 親設定可能な項目から子製品を削除

API を使用して cURL を使用して次のDELETEリクエストを送信することで、カタログから製品を削除せずに、設定可能な製品から子製品を削除できます。

次の例では、このメソッドをDELETEします。

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## その他のリソース

- [設定可能な製品チュートリアルの作成](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
- [設定可能な製品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html){target="_blank"}
- [Adobe Developer REST チュートリアル](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
