---
title: シンプルな商品の作成
description: REST APIとCommerce管理者を使用して、シンプルな製品を作成する方法を説明します。
kt: 14446
doc-type: video
duration: 197
audience: all
activity: use
last-substantial-update: 2023-11-14T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 62ba8e71-dcff-4c72-8850-029be2c42620
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# シンプルな商品の作成

REST APIとAdobe Commerce管理者を使用して、シンプルな製品を作成する方法を説明します。

## この動画は誰のためのものでしょうか？

* web サイトマネージャー
* コマースマーチャンダイジング
* REST APIを使用してAdobe Commerceでプロダクトを作成する方法を学習する新しいAdobe Commerce開発者。

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3443903?captions=jpn&learn=on)

## curlを使用した製品の作成

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

## curlを使用した製品の取得

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## 関連資料

* [Adobe Developer REST チュートリアル &#x200B;](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST文書](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
