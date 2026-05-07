---
title: Adobe Commerceでモジュールを作成してイベントを使用する方法を説明します。
description: Commerce モジュールを作成してイベントを使用する方法を説明します。
landing-page-description: Adobe Commerce モジュールを作成してイベントを使用する方法を説明します。
short-description: Adobe Commerce モジュールを作成してイベントを使用する方法を説明します。
kt: 11891
doc-type: tutorial
duration: 348
audience: all
last-substantial-update: 2023-02-21T00:00:00.000Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
TQID: https://experienceleague.adobe.com/bRnOh6fnsyTY-21f81vIXV4-eeitLXQWsAbjg2rx-Is
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 173
ht-degree: 0%

---

# Adobe Commerce モジュール開発

イベントの登録、サポートされているイベントの検索、カスタムモジュール開発で新しいXML ファイル `io_events.xml`を使用する方法について説明します。 また、このビデオでは、既に定義されている可能性のあるイベントを登録解除するだけでなく、使用できる登録済みイベントを見つける方法も開発者に示します。 その他のドキュメントについては、[Adobe Commerce用Adobe I/O Eventsのインストール &#x200B;](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* I/O イベントを使用して、Adobe CommerceとAdobe Developer App Builderを初めて使用する開発者。

## ビデオコンテンツ {#video-content}

* Adobe Developer App Builderで使用するためのCommerceでのイベントの登録
* 登録可能なイベントを特定する
* io_events.xmlでのイベントの登録方法を説明します
* Commerce インスタンス `app/etc/config.php`でイベントを登録する方法を説明します
* イベントの購読を解除する方法

>[!VIDEO](https://video.tv.adobe.com/v/3419837?captions=jpn&learn=on)

## 便利なコマンド {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}
