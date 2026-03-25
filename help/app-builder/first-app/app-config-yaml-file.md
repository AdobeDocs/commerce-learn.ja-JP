---
title: app.config.yaml ファイル
description: このサンプルアプリケーションのapp.config.yaml ファイル内のファイルの種類について説明します。
landing-page-description: Adobe Commerceで使用されるAdobe Developer App Builderと、app.config.yamlで使用されるファイルの種類について説明します。
kt: 12929
doc-type: tutorial
duration: 136
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: ff5f1811-ca93-494e-8e5c-a5e0c7bb673d
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# app.config.yaml ファイルの説明と使用 {#app-config-yaml}

このファイルは、アプリケーションの設定を決定します。

## この動画は誰のためのものでしょうか？

* Adobe Commerceを初めて使用する開発者で、Adobe App Builderの使用経験が限られており、サンプルアプリケーションの`app.config.yaml`について学んでいるユーザー。

## ビデオコンテンツ

* `app.config.yaml` ファイルについて説明しました
* 定義が他の`.js` ファイルにリンクする方法

>[!VIDEO](https://video.tv.adobe.com/v/3430839?captions=jpn&learn=on)

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

これらの静的値は、ファイル `actions/commerce.index.js`のサンプル モジュールで使用されています

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
