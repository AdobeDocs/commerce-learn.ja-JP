---
title: 設定可能な商品の作成
description: REST API とCommerce管理者を使用して、設定可能な商品を作成する方法について説明します。
kt: 14586
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 112bec9a-0f8e-4252-8c52-f486a5e663b5
source-git-commit: 765bf4159892416e02ea1e9b8e4fa69e396d40af
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---

# 設定可能な商品の作成

設定可能なプロダクトは、複数の単純なプロダクトの親プロダクトです。 設定可能な商品を定義し、購入者が 1 つ以上の選択肢を作成して特定の商品バリエーションを選択するように求めます。 例えば、商品がシャツの場合、購入者はシャツを選択するためにサイズと色のオプションを選択する必要があります。

設定可能な製品では、より多くの SKU を使用し、最初は設定に少し時間がかかる場合がありますが、最終的には時間を節約できます。 ビジネスの成長を計画している場合、複数のオプションを持つ製品には、設定可能な製品タイプが適しています。

設定可能な商品を作成する前に、設定可能な商品に含めるシンプルな商品がすべてAdobe Commerceで使用可能であることを確認します。 存在しないコンポーネントを作成します。

このチュートリアルでは、REST API とAdobe Commerce管理者を使用して設定可能な商品を作成する方法について説明します。

REST API を使用して、設定可能な製品を作成します。

1. [attribute set](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html?lang=ja) の属性を取得して、後続の API 呼び出しに ID 番号を使用します。
1. 設定可能な商品で使用するシンプルな商品を作成します。
1. 空の設定可能な商品を作成し、シンプルな商品を関連付けます。
1. 設定可能な製品の製品属性を設定します。
1. 空の設定可能な製品に簡単な製品を入力します。
1. 設定可能な製品とすべての属性を取得します。
1. 設定可能な製品に割り当てられた子製品を取得します。
1. シンプル製品と設定可能な製品の関連付けを削除します。

Adobe Commerce管理から設定可能な商品を作成する場合、最初にシンプルな商品を作成するか、ウィザードを使用して使用する新しいシンプルな商品を作成する自動ツールを使用できます。

## このビデオの目的は誰ですか。

- Web サイト管理者
- e コマースマーチャンダイザー
- Adobe Commerceの新しい開発者向けに、REST API を使用してAdobe Commerceで設定可能な商品を作成する方法を説明します

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3455036?learn=on&captions=jpn)

## cURL を使用したカラー属性の取得

この例では、属性セット 10 に対して、すべての個々の属性を含む属性セット全体が返されます。 それは長くすることができます、何百行も珍しくありません。 応答を確認する際、色の属性 ID が中央にある可能性があります。 grep またはその他のメソッドを使用して結果を検索することで、これらの値の検索を高速化します。 私の応答は 665 行目に近く、JSON 応答の次のスニペットに含まれています。

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


設定可能な製品を設定する属性 ID を取得するには、次の cURL リクエストの `attribute-sets/10/attributes` の部分を更新して、環境の属性セット ID`10` 置き換えます。 このリクエストでは、GETメソッドを使用します。

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## cURL を使用した最初のシンプルな製品の作成

### 環境 ID と製品詳細の調整

API を使用して最初のシンプルな商品を作成し、cURL を使用して次のPOSTリクエストを送信します。

リクエストを送信する前に、ご使用の環境の値で例を更新してください。

- `"attribute-set": 10` を変更して、`10` を環境の属性セット ID に置き換えます。
- `"value": "13"` を変更して、`13` をお使いの環境の値に置き換えます。

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

## cURL を使用して 2 つ目のシンプルな製品を作成します

API を使用して、cURL を使用して次のPOSTリクエストを送信することで、2 つ目のシンプルな商品を作成します。

リクエストを送信する前に、ご使用の環境の値で例を更新してください。

- `"attribute_set_id": 10,` を変更し、`10` を環境内の属性セット id に置き換えます。
- `"value": "14"` を変更し、`14` をお使いの環境の値に置き換えます。

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

## cURL を使用した 3 つ目のシンプルな製品の作成

cURL を使用して次のPOSTリクエストを送信し、3 つ目のシンプルな商品を作成します。

リクエストを送信する前に、ご使用の環境の値で例を更新してください。

- `"attribute_set_id": 10,` を変更して、`10` を環境の属性セット ID に置き換えます。
- `"value": "15"` を変更し、`15` をお使いの環境の値に置き換えます。

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

## cURL を使用して、空の設定可能な製品を作成

cURL を使用して次の設定リクエストを送信し、空のPOST可能な商品を作成します。

リクエストを送信する前に、ご使用の環境の値で例を更新してください。

- `"attribute_set_id": 10,` を変更し、`10` を環境の属性セット id に置き換えます。
- `"value": "93"` を変更し、`93` をお使いの環境の値に置き換えます。

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

## 設定可能な商品に使用可能なオプションの設定

cURL を使用して以下の設定リクエストを送信して、設定可能な商品に使用可能なPOSTを指定します。

リクエストを送信する前に、`"attribute_id": 93,` を変更して、`93` を環境の属性 id に置き換えます。

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

設定可能な製品（親）のオプションの設定を忘れると、設定可能な製品に子の製品を関連付けようとすると、エラーが発生します。 エラーメッセージは、次の例のようになります。

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## 子製品を設定可能な製品にリンク

これで、3 つのシンプルな製品が作成されました。

- `"Kids Hawaiian Ukulele Red"`,
- `"Kids-Hawaiian-Ukulele-Blue"`
- `"Kids-Hawaiian-Ukulele-Green"`

次のPOSTリクエストを送信して、これらの単純な商品を設定可能な商品の子として追加します。 製品ごとに個別のリクエストを送信します。

各リクエストで、`childSKU` の値を、追加する子製品の値に更新します。 次の例では、単純な製品 `kids-Hawaiian-Ukulele-red` を SKU`Kids-Hawaiian-Ukulele-red` を持つ設定可能な製品に割り当てます。


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

これで、3 つの子 SKU が割り当てられた、設定可能な製品を作成しました。 cURL を使用して次のGETリクエストを送信すると、割り当てられた商品のリンクされた ID を確認できます。 このリクエストは、設定可能な商品に関する詳細情報を返します。

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

以下では、GETメソッドを使用します

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 設定可能な商品に関連付けられた子商品を取得します

次のGETリクエストを送信して、設定可能な商品に関連付けられている子のみを返します。 応答には、SKU や価格など、子製品のすべての属性が含まれます。

以下では、GETメソッドを使用します

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 親設定可能なものから子製品を削除または削除

cURL を使用して次の削除リクエストを送信することで、カタログから商品を削除することなく、設定可能な商品から子商品をDELETEできます。

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## その他のリソース

- [ 設定可能な製品の作成チュートリアル ](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
- [ 設定可能な製品 ](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html?lang=ja){target="_blank"}
- [Adobe Developer REST チュートリアル ](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
