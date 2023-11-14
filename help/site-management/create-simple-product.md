---
title: シンプルな製品の作成
description: REST API とコマース管理を使用して簡単な製品を作成する方法を説明します。
kt: 14446
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-14T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: 89dc3b7f456c9434921ed870369712a721895d02
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 0%

---

# シンプルな製品の作成

REST API とAdobe Commerce Admin を使用して簡単な製品を作成する方法を説明します。

## このビデオは誰のためのものですか？

- Web サイトマネージャー
- e コマースマーチャンダイザー
- REST API を使用してAdobe Commerceで製品を作成する方法を学びたい新しいAdobe Commerce開発者です。

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3425650?learn=on)

## curl を使用した製品の作成

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "some-product-sku",
    "name": "My curl created product test",
    "attribute_set_id": 4,
    "price": 222,
    "type_id": "simple"
  }
}
```

## curl を使用した製品の取得

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## その他のリソース

- [Adobe Developer REST チュートリアル](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
