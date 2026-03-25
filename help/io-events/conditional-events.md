---
title: Adobe Commerceで条件付きイベントを使用する方法を説明します
description: Adobe Developer App Builderで使用するコンディショナルイベントの使用方法を説明します。
landing-page-description: Adobe Commerceの条件イベントの使用方法を説明します。
short-description: Adobe Commerceの条件イベントの使用方法を説明します。
kt: 11890
doc-type: tutorial
duration: 421
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Adobe Commerce条件イベント

Adobe Developer App Builderで使用できるAdobe Commerceの条件付きイベントについて説明します。 その他のドキュメントについては、[Adobe Commerce用Adobe I/O Eventsのインストール ](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* I/O イベントを使用してAdobe CommerceとAdobe Developer App Builderを初めて使用する開発者は、Adobe App Builder プロジェクトを作成する必要があります。

## ビデオコンテンツ {#video-content}

* 条件付きイベントについて詳しく見る
* 新しいXML ファイル io_events.xmlの適切な使用方法を説明します
* 条件付きイベントを設定する方法を説明します
* 条件付きイベントで使用するルールの定義
* Commerce インスタンス `app/etc/config.php`でイベントを登録する方法を説明します

>[!VIDEO](https://video.tv.adobe.com/v/3415806?learn=on)

## 便利なコマンド {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
