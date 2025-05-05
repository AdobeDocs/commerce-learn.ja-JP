---
title: Adobe Commerceで条件付きイベントを使用する方法を説明します
description: Adobe Developer App Builderで使用する条件付きイベントの使用方法を説明します。
landing-page-description: Adobe Commerceの条件付きイベントの使用方法を説明します。
short-description: Adobe Commerceの条件付きイベントの使用方法を説明します。
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: e02da0901c01871360bcc556666e310acd7c7224
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Adobe Commerce条件付きイベント

Adobe Developer App Builderで使用できるAdobe Commerceの条件付きイベントについて説明します。 追加ドキュメントについては、[Adobe CommerceのAdobe I/Oイベントのインストール ](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"} を参照してください。

## このビデオの目的は誰ですか。

* I/O イベントを使用してAdobe CommerceとAdobe Developer App Builderを初めて使用し、AdobeのApp Builder プロジェクトを作成する必要がある開発者。

## ビデオコンテンツ {#video-content}

* 条件付きイベントについて学ぶ
* 新しい XML ファイル io_events.xml の適切な使用方法を説明します
* 条件付きイベントの設定方法を学ぶ
* 条件付きイベントで使用するルールの定義
* Commerce インスタンスでイベントを登録する方法を説明します `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3419801?quality=12&learn=on&captions=jpn)

## 便利なコマンド {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
