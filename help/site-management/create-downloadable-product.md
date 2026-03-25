---
title: ダウンロード可能な製品を作成する
description: REST APIとAdobe Commerce管理者を使用して、ダウンロード可能な製品を作成する方法を説明します。
kt: 14464
doc-type: video
duration: 946
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# ダウンロード可能な製品を作成する

REST APIとAdobe Commerce管理者を使用して、ダウンロード可能な製品を作成する方法を説明します。

## この動画は誰のためのものでしょうか？

* web サイトマネージャー
* コマースマーチャンダイジング
* REST APIを使用してAdobe Commerceでプロダクトを作成する方法を学びたい新しいAdobe Commerce開発者

## ビデオコンテンツ

>[!VIDEO](https://video.tv.adobe.com/v/3425753?learn=on)

## ダウンロード可能なドメイン

ダウンロードを許可するドメインを指定してください。 ドメインは、コマンドラインを使用してプロジェクトの`env.php` ファイルに追加されます。 `env.php` ファイルには、ダウンロード可能なコンテンツを含めることができるドメインの詳細が記載されています。 REST API _を使用してダウンロード可能な製品を作成すると、_ ファイルが更新される前にエラーが発生します。`php.env`

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

ドメインを設定するには、サーバーに接続してください：`bin/magento downloadable:domains:add www.example.com`

これが完了すると、`env.php`は&#x200B;_downloadable_domains_&#x200B;配列内で変更されます。

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

ドメインが`env.php`に追加されたので、Adobe Commerce AdminまたはREST APIを使用して、ダウンロード可能な製品を作成できます。

詳しくは、[構成参照](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=ja#downloadable_domains)を参照してください。

>[!IMPORTANT]
>Adobe Commerceの一部のバージョンでは、Adobe Commerce管理者で商品を編集すると、次のエラーが表示される場合があります。 製品はREST APIを使用して作成されますが、リンクされたダウンロードの価格は`null`です。

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`。

このエラーを修正するには、更新リンク APIを使用します：`POST V1/products/{sku}/downloadable-links.`

詳しくは、「[cURLを使用して製品ダウンロードリンクを更新する](#update-downloadable-links)」の節を参照してください。

## cURLを使用してダウンロード可能な製品を作成する（リモートサーバーからダウンロード）

この例では、ダウンロードするファイルが同じサーバー上にない場合に、cURLを使用してダウンロード可能な製品を作成する方法を示します。 この使用例は、ファイルがS3 バケットまたはその他のデジタルアセット管理に保存されている場合に一般的です。

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

## cURLを使用してダウンロード可能な製品を作成する（Commerce アプリケーションサーバーからダウンロード）

この例では、ファイルがAdobe Commerce アプリケーションと同じサーバーに保存されている場合に、cURLを使用してAdobe Commerce管理者からダウンロード可能な製品を作成する方法を示します。

この使用例では、カタログを管理する管理者が`upload file`を選択すると、ファイルが`pub/media/downloadable/files/links/` ディレクトリに転送されます。  オートメーションは、次のパターンに基づいてファイルを作成し、それぞれの場所に移動します。

* アップロードされた各ファイルは、ファイル名の最初の2文字に基づいてフォルダーに保存されます。
* アップロードが開始されると、Commerce アプリケーションは既存のフォルダーを作成または使用してファイルを転送します。
* ファイルをダウンロードする際、パスの`link_file` セクションは、`pub/media/downloadable/files/links/` ディレクトリに追加されたパスの部分を使用します。

例えば、アップロードされたファイルの名前が`download-example.zip`の場合は次のようになります。

* ファイルがパス `pub/media/downloadable/files/links/d/o/`にアップロードされます。
サブディレクトリ `/d`と`/d/o`が存在しない場合は作成されます。

* ファイルへの最終パスは`/pub/media/downloadable/files/links/d/o/download-example.zip`です。

* この例の`link_url`値は`d/o/download-example.zip`です

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

## curlを使用した製品の取得

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## Postmanを使用した製品の更新 {#update-downloadable-links}

エンドポイント `rest/all/V1/products/{sku}/downloadable-links`を使用
`SKU`は、製品の作成時に生成された製品IDです。 例えば、以下のコードサンプルでは、番号は39ですが、web サイトのIDを使用するように更新されていることを確認してください。 これにより、ダウンロード可能な製品のリンクが更新されます。

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

## CURLを使用した製品ダウンロードリンクの更新

cURLを使用して製品ダウンロードリンクを更新すると、URLには更新中の製品のSKUが含まれます。  次のコード例では、SKUは`abcd12345`です。 コマンドを送信する場合は、更新する製品のSKUに合わせて値を変更します。

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

## 関連資料

* [&#x200B; ダウンロード可能な製品タイプ &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=ja){target="_blank"}
* [&#x200B; ダウンロード可能なドメイン設定ガイド &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=ja#downloadable_domains){target="_blank"}
* [Adobe Developer REST チュートリアル &#x200B;](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST文書](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
