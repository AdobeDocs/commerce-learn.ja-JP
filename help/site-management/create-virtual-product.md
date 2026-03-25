---
title: バーチャル商品の作成
description: REST APIとCommerce Adminを使用してバーチャル製品を作成する方法を説明します。
kt: 14464
doc-type: video
duration: 213
audience: all
activity: use
last-substantial-update: 2023-11-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 5149b6b4-5fbf-467a-a412-6dce7188bcb9
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 0%

---

# バーチャル商品の作成

REST APIとAdobe Commerce Adminを使用してバーチャル製品を作成する方法を説明します。

## この動画は誰のためのものでしょうか？

* web サイトマネージャー
* コマースマーチャンダイジング
* REST APIを使用してAdobe Commerceでプロダクトを作成する方法を学習する新しいAdobe Commerce開発者。

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3444865?captions=jpn&learn=on)

## curlを使用したバーチャル商品の作成

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

## curlを使用した製品の取得

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Admin-created-virtual-product' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=2cd97649bcf4ee5b45b23f8eb940e3a0; private_content_version=564dde2976849891583a9a649073f01e'
```

## 関連資料

* [Adobe Developer REST チュートリアル &#x200B;](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST文書](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
