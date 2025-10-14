---
title: ダウンロード可能な製品を作成
description: REST API とAdobe Commerce Admin を使用して、ダウンロード可能な商品を作成する方法を説明します。
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: eba043cd4169cd762653557bf9283b8d6a208ef0
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# ダウンロード可能な製品を作成

REST API とAdobe Commerce Admin を使用して、ダウンロード可能な商品を作成する方法を説明します。

## このビデオの目的は誰ですか。

- Web サイト管理者
- e コマースマーチャンダイザー
- Adobe Commerceの新規開発者向けに、REST API を使用してAdobe Commerceで商品を作成する方法を説明します

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3453942?learn=on&captions=jpn)

## 許可されているダウンロード可能ドメイン

ダウンロードを許可するドメインを指定してください。 ドメインは、コマンドラインを使用してプロジェクトの `env.php` ファイルに追加されます。 `env.php` ファイルは、ダウンロード可能なコンテンツを含めることができるドメインの詳細を示します。 ダウンロード可能な製品が REST API を使用して作成された場合 _以前_、`php.env` ファイルが更新されると、エラーが発生します。

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

ドメインを設定するには、サーバーに接続します：`bin/magento downloadable:domains:add www.example.com`

これが完了すると、`env.php` は _downloadable_domains_ 配列内で変更されます。

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

ドメインが `env.php` に追加されたので、Adobe Commerce管理者または REST API を使用して、ダウンロード可能な商品を作成できます。

詳しくは、[&#x200B; 設定リファレンス &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=ja#downloadable_domains) を参照してください。

>[!IMPORTANT]
>Adobe Commerceの一部のバージョンでは、Adobe Commerce管理者で商品を編集すると、次のエラーが発生することがあります。 製品は REST API を使用して作成されますが、リンクされたダウンロードの価格は `null` です。

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`。

このエラーを修正するには、リンク更新 API を使用します：`POST V1/products/{sku}/downloadable-links.`

詳しくは、[cURL を使用した製品ダウンロードリンクのアップデート &#x200B;](#update-downloadable-links) の節を参照してください。

## cURL を使用してダウンロード可能な製品を作成する（リモートサーバーからダウンロード）

次の例では、ダウンロードするファイルが同じサーバー上にない場合に、cURL を使用してダウンロード可能な製品を作成する方法を示します。 これは、ファイルが S3 バケットまたはその他のデジタルアセットマネージャーに格納されている場合の典型的なユースケースです。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01' \
--data '{
  "product": {
    "sku": "POSTMAN-download-product-1",
    "name": "POSTMAN download product-1",
    "attribute_set_id": 4,
    "price": 9.9,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        
        "downloadable_product_links": [
            {
                "title": "My url link",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "url",
                "link_url": "{{location.url.where.file.exists}}/some-file.zip",
                "sample_type": "url",
                "sample_url": "{{location.url.where.file.exists}}/sample-file.zip"
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}
'
```

## cURL を使用してダウンロード可能な製品を作成する（Commerce アプリケーションサーバーからダウンロード）

この例では、ファイルがAdobe Commerce アプリケーションと同じサーバーに保存されている場合に、cURL を使用してAdobe Commerce Admin からダウンロード可能な商品を作成する方法を示しています。

このユースケースでは、カタログを管理する管理者が `upload file` を選択すると、ファイルは `pub/media/downloadable/files/links/` ディレクトリに転送されます。  自動化では、次のパターンに基づいてファイルが作成され、それぞれの場所に移動されます。

- アップロードされた各ファイルは、ファイル名の最初の 2 文字に基づいてフォルダーに保存されます。
- アップロードが開始されると、Commerce アプリケーションは既存のフォルダーを作成するか、使用してファイルを転送します。
- ファイルのダウンロード時に、パスの `link_file` セクションは、`pub/media/downloadable/files/links/` ディレクトリに追加されたパスの部分を使用します。

例えば、アップロードされたファイルの名前が `download-example.zip` の場合は、次のようになります。

- ファイルがパス `pub/media/downloadable/files/links/d/o/` にアップロードされます。
サブディレクトリ `/d` および `/d/o` は、存在しない場合に作成されます。

- ファイルの最終パスは `/pub/media/downloadable/files/links/d/o/download-example.zip` です。

- この例の `link_url` 値は `d/o/download-example.zip` です

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=571f5ebe48857569cd56bde5d65f83a2; private_content_version=9f3286b427812be6aec0b04cae33ba35' \
--data '{
  "product": {
    "sku": "sample-local-download-file",
    "name": "A downloadable product with locally hosted download file",
    "attribute_set_id": 4,
    "price": 33,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        "downloadable_product_links": [
            {
                "title": "an api version of already upload file",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "file",
                "link_file": "/d/o/downloadable-file-demo-file-upload.zip",
                "sample_type": null
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}'
```

## curl を使用した製品の取得

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## Postmanを使用した製品のアップデート {#update-downloadable-links}

エンドポイント `rest/all/V1/products/{sku}/downloadable-links` の使用
`SKU` は、製品の作成時に生成された製品 ID です。 例えば、以下のコードサンプルでは、この値は 39 ですが、web サイトの ID を使用するように更新してください。 これにより、ダウンロード可能な製品のリンクが更新されます。

```json
{
  "link": {
    "id": 39,
    "title": "My swagger update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}
```

## CURL を使用した製品ダウンロードリンクのアップデート

cURL を使用して製品のダウンロードリンクを更新すると、その URL には、更新中の製品の SKU が含まれます。  次のコードの例では、SKU は `abcd12345` です。 コマンドを送信したら、値を更新する製品の SKU に一致するように変更します。

```bash
curl --location '{{your.url.here}}/rest/all/V1/products/abcd12345/downloadable-links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=fa5d76f4568982adf369f758e8cb9544; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "link": {
    "id": {{The ID of the download link for example 39}},
    "title": "My update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}'
```

## その他のリソース

- [&#x200B; ダウンロード可能な製品タイプ &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=ja){target="_blank"}
- [&#x200B; ダウンロード可能なドメインの設定ガイド &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=ja#downloadable_domains){target="_blank"}
- [Adobe Developer REST チュートリアル &#x200B;](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
