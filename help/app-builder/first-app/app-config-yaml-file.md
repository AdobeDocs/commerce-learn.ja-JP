---
title: app.config.yaml ファイル
description: app.config.yaml ファイルがアプリケーション設定を決定する方法と、その定義がAdobe Developer App Builder サンプルアプリケーションのJavaScript ファイルにどのようにリンクされるかを説明します。
jira: KT-12929
doc-type: Tutorial
duration: 136
last-substantial-update: 2023-03-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: ff5f1811-ca93-494e-8e5c-a5e0c7bb673d
TQID: https://experienceleague.adobe.com/iK4PPaI2-vxQK32DMfkMRZMgNYpLExMbNge2lXIJzLg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 282072f1e29b836d19dee2e1b6498f75150fe3a5
workflow-type: tm+mt
source-wordcount: 96
ht-degree: 0%

---

# app.config.yaml ファイルの説明と使用 {#app-config-yaml}

このファイルは、アプリケーションの設定を決定します。

## この動画は誰のためのものでしょうか？

* Adobe Commerceを初めて使用する開発者で、Adobe App Builderの使用経験が限られており、サンプルアプリケーションの`app.config.yaml`について学んでいるユーザー。

## ビデオコンテンツ

* `app.config.yaml` ファイルについて説明しました
* 他の`.js` ファイルに定義をリンクする方法

>[!VIDEO](https://video.tv.adobe.com/v/3416592?learn=on)

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


