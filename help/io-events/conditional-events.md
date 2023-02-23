---
title: Adobe Commerceでの条件付きイベントの使用方法を説明します
description: Adobe Developer App Builder で使用する条件付きイベントの使用方法を説明します。
landing-page-description: Adobe Commerceの条件付きイベントの使用方法を説明します。
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: f5de9191315c7df2eaca4ee51d03b30e2a2fac99
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---


# Adobe Commerce条件付きイベント

Adobe Developer App Builder で使用できるAdobe Commerceの条件付きイベントについて説明します。 その他のドキュメントは、 [Adobe CommerceのAdobe I/Oイベントのインストール](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## このビデオは誰のためのものですか？

* I/O イベントを使用したAdobe CommerceおよびAdobe Developer App Builder を初めて使用する開発者は、アプリビルダープロジェクトをAdobeする必要があります。

## ビデオコンテンツ {#video-content}

* 条件付きイベントの詳細
* 新しい XML ファイル io_events.xml の適切な使用方法を説明します。
* 条件付きイベントの設定方法を説明します
* 条件付きイベントで使用するルールの定義
* コマースインスタンスにイベントを登録する方法を説明します `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3415806)

## 便利なコマンド {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
