---
title: イベントを使用するためのAdobe Commerceでモジュールを作成する方法を説明します。
description: イベントを使用するコマースモジュールの作成方法を説明します。
landing-page-description: イベントを使用するAdobe Commerceモジュールの作成方法を説明します。
short-description: イベントを使用するAdobe Commerceモジュールの作成方法を説明します。
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# Adobe Commerceモジュールの開発

イベントの登録方法、サポートされるイベントの検索方法、新しい XML ファイルの使用方法について説明します。 `io_events.xml` （カスタムモジュール開発）。 また、このビデオでは、使用可能な登録イベントを見つける方法や、既に定義されているイベントを登録解除する方法についても開発者に説明します。 その他のドキュメントは、 [Adobe CommerceのAdobe I/Oイベントのインストール](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## このビデオは誰のためのものですか？

* I/O イベントを使用した、Adobe CommerceとAdobe Developer App Builder を初めて利用する開発者。

## ビデオコンテンツ {#video-content}

* Adobe Developer App Builder で使用する Commerce でのイベントの登録
* 登録可能なイベントの特定
* io_events.xml にイベントを登録する方法を説明します。
* コマースインスタンスにイベントを登録する方法を説明します `app/etc/config.php`
* イベントの購読解除方法を説明します

>[!VIDEO](https://video.tv.adobe.com/v/3415802?quality=12&learn=on)

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
