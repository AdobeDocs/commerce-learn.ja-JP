---
title: イベントを使用するモジュールをAdobe Commerceで作成する方法を説明します。
description: イベントを使用するCommerce モジュールを作成する方法を説明します。
landing-page-description: イベントを使用するAdobe Commerce モジュールを作成する方法について説明します。
short-description: イベントを使用するAdobe Commerce モジュールを作成する方法について説明します。
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Adobe Commerce module development

カスタムモジュール開発で、イベントを登録、サポートされるイベントを検索、新しい XML ファイルを使用す `io_events.xml` 方法について説明します。 このビデオでは、使用できる登録済みイベントの検索方法や、既に定義されているイベントを登録解除する方法も開発者に示します。 追加ドキュメントについては、[Adobe CommerceのAdobe I/Oイベントのインストール &#x200B;](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"} を参照してください。

## このビデオの目的は誰ですか。

* I/O イベントを使用してAdobe CommerceとAdobe Developer App Builderを初めて使用する開発者。

## ビデオコンテンツ {#video-content}

* Adobe Developer App Builderで使用するCommerceでのイベントの登録
* 登録できるイベントの識別
* io_events.xml にイベントを登録する方法を学ぶ
* Commerce インスタンスでイベントを登録する方法を説明します `app/etc/config.php`
* イベントの配信停止について説明します

>[!VIDEO](https://video.tv.adobe.com/v/3419837?quality=12&learn=on&captions=jpn)

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
