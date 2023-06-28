---
title: .env ファイル
description: このサンプルアプリケーションの.env ファイル内のファイルの種類について説明します
landing-page-description: Adobe Commerceで使用されるAdobe Developer App Builder と、.env ファイルで使用されるコンテンツの種類について説明します
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# .env ファイルの生成と設定 {#env-file}

この `.env` は、サンプルモジュールには含まれていませんが、Adobe Developer App Builder アプリケーションでの使用に重要な特別なファイルです。 このファイルには、秘密鍵とその他の情報が含まれています。 このファイルをコードリポジトリにコミットしないでください。

## このビデオは誰のためのものですか？

* Adobe Commerceを初めて使用する開発者で、Adobeの App Builder を使用して、 `.env` ファイル。

## ビデオコンテンツ

* .env ファイルの概要とその目的
* .env ファイルの生成方法
* ファイルを追加して新しい秘密鍵を追加する方法
* このファイルには機密情報が含まれているので、このファイルをコミットしないでください

>[!VIDEO](https://video.tv.adobe.com/v/3416593?quality=12&learn=on)

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

これらの静的値は、ファイルのサンプルモジュールで使用されています `actions/commerce.index.js`.

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
