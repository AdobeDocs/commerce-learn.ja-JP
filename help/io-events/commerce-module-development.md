---
title: Adobe Commerceでモジュールを作成してイベントを使用する方法を説明します。
description: Commerce モジュールを作成してイベントを使用する方法を説明します。
landing-page-description: Adobe Commerce モジュールを作成してイベントを使用する方法を説明します。
short-description: Adobe Commerce モジュールを作成してイベントを使用する方法を説明します。
kt: 11891
doc-type: tutorial
duration: 348
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Adobe Commerce モジュール開発

イベントの登録、サポートされているイベントの検索、カスタムモジュール開発で新しいXML ファイル `io_events.xml`を使用する方法について説明します。 また、このビデオでは、既に定義されている可能性のあるイベントを登録解除するだけでなく、使用できる登録済みイベントを見つける方法も開発者に示します。 その他のドキュメントについては、[Adobe Commerce用Adobe I/O Eventsのインストール ](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* I/O イベントを使用して、Adobe CommerceとAdobe Developer App Builderを初めて使用する開発者。

## ビデオコンテンツ {#video-content}

* Adobe Developer App Builderで使用するためのCommerceでのイベントの登録
* 登録可能なイベントを特定する
* io_events.xmlでのイベントの登録方法を説明します
* Commerce インスタンス `app/etc/config.php`でイベントを登録する方法を説明します
* イベントの購読を解除する方法

>[!VIDEO](https://video.tv.adobe.com/v/3415802?learn=on)

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
