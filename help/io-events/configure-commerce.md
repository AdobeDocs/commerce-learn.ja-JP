---
title: Adobe Commerceの設定
description: Adobe Developer App Builderでイベントを使用できるようにAdobe Commerceを設定する方法を説明します。
jira: KT-11889
doc-type: Tutorial
duration: 268
last-substantial-update: 2023-02-21
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
TQID: https://experienceleague.adobe.com/6FBcDMDst5LBS7EyqA8zINr2Vs3bC--M7CXzIKf6EeQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 9f50b87d13f48b239d814783eb2c56319946cb29
workflow-type: tm+mt
source-wordcount: 114
ht-degree: 0%

---

# Adobe Commerceの設定

イベントを公開するようにAdobe Commerceを設定する方法を説明します。 その他のドキュメントについては、[Adobe Commerce用Adobe I/O Eventsのインストール &#x200B;](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* Adobe App Builder プロジェクトを作成する必要があるI/O イベントを使用して、Adobe CommerceとAdobe Developer App Builderを初めて使用する開発者。

## ビデオコンテンツ {#video-content}

* Commerce管理者でのAdobe I/O Eventsの設定
* Commerce管理画面での秘密鍵の保存
* 一意のIDをCommerce管理者に保存する
* イベントプロバイダーの作成

>[!VIDEO](https://video.tv.adobe.com/v/3415799?learn=on)

## 便利なコマンド {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}

