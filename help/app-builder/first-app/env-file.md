---
title: .env ファイル
description: このサンプルアプリケーションの.env ファイルのファイルの種類について説明します
landing-page-description: Adobe Commerceで使用されるAdobe Developer App Builderと、.env ファイルで使用されるコンテンツの種類について説明します
kt: 12423
doc-type: tutorial
duration: 177
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# .env ファイルの生成と設定 {#env-file}

`.env`は、サンプルモジュールに含まれていないが、Adobe Developer App Builder アプリケーションで使用する際に重要な特殊ファイルです。 このファイルには、秘密鍵とその他の情報が含まれています。 このファイルを任意のコードリポジトリにコミットしないでください。

## この動画は誰のためのものでしょうか？

* Adobe Commerceを初めて使用する開発者で、Adobe App Builderを使用した経験が限られており、`.env` ファイルについて学習する必要があります。

## ビデオコンテンツ

* .env ファイルとその目的の概要
* .env ファイルの生成方法
* ファイルを追加して新しいシークレットを追加する方法
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
