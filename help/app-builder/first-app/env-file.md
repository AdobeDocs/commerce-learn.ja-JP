---
title: .env ファイル
description: シークレットの管理やソース管理への誤ったコミットの防止など、Adobe Developer App Builder アプリケーションの.env ファイルを生成および設定する方法について説明します。
jira: KT-21681
doc-type: Tutorial
duration: 177
last-substantial-update: 2023-03-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
TQID: https://experienceleague.adobe.com/Sup5quVNU60RYmkCJaYGgLq2gHCpCvaIKEFLH2MAifQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3f243b43695e65d63cb25142ec5a886ce390d502
workflow-type: tm+mt
source-wordcount: 147
ht-degree: 0%

---

# .env ファイルの生成と設定 {#env-file}

`.env`は、サンプルモジュールに含まれていないが、Adobe Developer App Builder アプリケーションで使用する際に重要な特殊ファイルです。 このファイルには、秘密鍵とその他の情報が含まれています。 このファイルを任意のコードリポジトリにコミットしないでください。

## この動画は誰のためのものでしょうか？

* Adobe Commerceを初めて使用する開発者で、Adobe App Builderを使用した経験が限られており、`.env` ファイルについて学習する必要があります。

## ビデオコンテンツ

* .env ファイルとその目的の概要
* .env ファイルの生成方法
* 新しいシークレットを追加するには、ファイルを追加します
* このファイルには機密情報が含まれているため、コミットしないでください

>[!VIDEO](https://video.tv.adobe.com/v/3416593?learn=on)

## コードサンプル

```bash
# Specify your secrets here
# The .env file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can include some commerce OAUTH credentials too, our sample module will use this
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

これらの静的値は、ファイル `actions/commerce.index.js`のサンプル モジュールで使用されています。

```javascript
        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}

