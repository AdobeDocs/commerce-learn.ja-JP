---
title: 仮想製品の作成
description: REST API とコマース管理を使用して仮想製品を作成する方法を説明します。
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: 225faceffefc31a6205f689933210510dba235d1
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# 仮想製品の作成

REST API とAdobe Commerce Admin を使用して仮想製品を作成する方法を説明します。

## このビデオは誰のためのものですか？

- Web サイトマネージャー
- e コマースマーチャンダイザー
- REST API を使用してAdobe Commerceで製品を作成する方法を学びたい新しいAdobe Commerce開発者です。

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3425723?learn=on)

## curl を使用した仮想製品の作成

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=2cd97649bcf4ee5b45b23f8eb940e3a0; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "postman-rest-virtual-product-test",
    "name": "POSTMAN virtual product test",
    "attribute_set_id": 4,
    "price": 44,
    "type_id": "virtual"
  }
}
'
```

## curl を使用した製品の取得

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Admin-created-virtual-product' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=2cd97649bcf4ee5b45b23f8eb940e3a0; private_content_version=564dde2976849891583a9a649073f01e'
```

## その他のリソース

- [Adobe Developer REST チュートリアル](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
