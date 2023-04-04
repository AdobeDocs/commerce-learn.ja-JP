---
title: app.config.yaml ファイル
description: このサンプルアプリケーションの app.config.yaml ファイル内のファイルの種類について説明します。
landing-page-description: Adobe Commerceで使用されるAdobe Developer App Builder と、app.config.yaml に含まれるファイルの種類について説明します。
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---


# app.config.yaml ファイルの説明と使用方法 {#app-config-yaml}

このファイルは、アプリケーションの設定を決定します。

## このビデオは誰のためのものですか？

* Adobe Commerceを初めて使用する開発者で、Adobeの App Builder を使用した経験が限られており、 `app.config.yaml` （サンプルアプリケーション内）

## ビデオコンテンツ

* この `app.config.yaml` ファイルを議論した
* 定義は他の定義とどのようにリンクしているか `.js` ファイル

>[!VIDEO](https://video.tv.adobe.com/v/3416592?quality=12&learn=on)

## コードサンプル

```bash
# Specify your secrets here
# This file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can also include commerce OAuth credentials, our sample module will use the following example credentials:
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

これらの静的値は、ファイルのサンプルモジュールで使用されています `actions/commerce.index.js`

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
