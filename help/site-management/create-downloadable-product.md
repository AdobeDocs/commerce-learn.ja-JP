---
title: ダウンロード可能な製品の作成
description: REST API とAdobe Commerce Admin を使用してダウンロード可能な製品を作成する方法を説明します。
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: 043d873e9b649455202de9df137c7283d92a2a4a
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# ダウンロード可能な製品の作成

REST API とAdobe Commerce Admin を使用してダウンロード可能な製品を作成する方法を説明します。

## このビデオは誰のためのものですか？

- Web サイトマネージャー
- e コマースマーチャンダイザー
- REST API を使用してAdobe Commerceで製品を作成する方法を学びたい新しいAdobe Commerce開発者

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3425753?learn=on)

## 許可されるダウンロード可能ドメイン

ダウンロードを許可するドメインを指定する必要があります。 ドメインがプロジェクトの `env.php` ファイルを指定します。 The `env.php` ファイルには、ダウンロード可能なコンテンツを含めることが許可されたドメインの詳細が記載されています。 REST API を使用してダウンロード可能な製品が作成された場合、エラーが発生します _前_  の `php.env` ファイルが更新されました：

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

ドメインを設定するには、次の手順でサーバーに接続します。 `bin/magento downloadable:domains:add www.example.com`

これが完了したら、 `env.php` が _downloadable_domains_ 配列。

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

これで、ドメインが `env.php`を使用する場合は、Adobe Commerce Admin で、または REST API を使用して、ダウンロード可能な製品を作成できます。

詳しくは、 [設定リファレンス](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html#downloadable_domains) を参照してください。 詳しくは、 [Adobe Commerce向け CLI リファレンス]( 詳しくは、https://experienceleague.adobe.com/docs/commerce-operations/reference/magento-open-source.html#downloadable%3Adomains%3Aaddを参照してください。

>[!IMPORTANT]
>Adobe Commerceの一部のバージョンでは、製品をAdobe Commerce Admin で編集すると、次のエラーが発生することがあります。 製品は REST API を使用して作成されますが、リンクされたダウンロードには `null` 価格。

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`.

このエラーを修正するには、更新リンク API を使用します。 `POST V1/products/{sku}/downloadable-links.`

詳しくは、 [cURL を使用した製品ダウンロードリンクの更新](#update-downloadable-links) 」セクションを参照してください。

## cURL を使用してダウンロード可能な製品を作成する（リモートサーバーからダウンロード）

この例では、ダウンロードするファイルが同じサーバー上にない場合に、cURL を使用してダウンロード可能な製品を作成する方法を示します。 この使用例は、ファイルが S3 バケットまたは他の Digital Asset Manager に保存されている場合に典型的です。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e' \
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

この例では、ファイルがAdobe Commerceアプリケーションと同じサーバーに保存されている場合に、cURL を使用してAdobe Commerce Admin からダウンロード可能な製品を作成する方法を示します。

この使用例では、カタログを管理する管理者が選択する場合に、 `upload file`に値を指定しない場合、ファイルは `pub/media/downloadable/files/links/` ディレクトリ。  自動化は、次のパターンに基づいて、ファイルを作成し、それぞれの場所に移動します。

- アップロードされた各ファイルは、ファイル名の最初の 2 文字に基づいてフォルダーに保存されます。
- アップロードが開始されると、Commerce アプリケーションは既存のフォルダーを作成または使用してファイルを転送します。
- ファイルをダウンロードする際に、 `link_file` パスのセクションは、パスの `pub/media/downloadable/files/links/` ディレクトリ。

例えば、アップロードされたファイルの名前が `download-example.zip`:

- ファイルはパスにアップロードされます `pub/media/downloadable/files/links/d/o/`.
サブディレクトリ `/d` および `/d/o` 作成されます（存在しない場合）。

- ファイルの最終パスはです。 `/pub/media/downloadable/files/links/d/o/download-example.zip`.

- The `link_url` この例の値は次のとおりです。 `d/o/download-example.zip`

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

エンドポイントを使用 `rest/all/V1/products/{sku}/downloadable-links`
The `SKU` は、製品が作成された際に生成された製品 ID です。 例えば、以下のコードサンプルでは 39 という数字ですが、Web サイトの ID を使用するように更新されていることを確認します。 これにより、ダウンロード可能な製品のリンクが更新されます。

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

## CURL を使用した製品ダウンロードリンクの更新

cURL を使用して製品のダウンロードリンクを更新すると、URL には更新中の製品の SKU が含まれます。  次のコード例では、SKU は `abcd12345`. コマンドを送信する際に、更新する製品の SKU に合わせて値を変更します。

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

- [ダウンロード可能な製品タイプ](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html){target="_blank"}
- [ダウンロード可能ドメイン設定ガイド](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html#downloadable_domains){target="_blank"}
- [.env.php のダウンロード可能なドメインへの追加](https://experienceleague.adobe.com/docs/commerce-operations/reference/magento-open-source.html#downloadable%3Adomains%3Aadd){target="_blank}
- [Adobe Developer REST チュートリアル](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
