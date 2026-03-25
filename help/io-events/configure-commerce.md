---
title: Adobe Commerceの設定
description: Adobe Developer App Builderでイベントを使用できるようにAdobe Commerceを設定する方法を説明します。
landing-page-description: Adobe Developer App Builderで使用するイベントメカニズムを使用するようにAdobe Commerceを設定する方法を説明します。
short-description: Adobe Developer App Builderで使用するイベントメカニズムを使用するようにAdobe Commerceを設定する方法を説明します。
kt: 11889
doc-type: tutorial
duration: 299
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Adobe Commerceの設定

イベントを公開するようにAdobe Commerceを設定する方法を説明します。 その他のドキュメントについては、[Adobe Commerce用Adobe I/O Eventsのインストール &#x200B;](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}を参照してください。

## この動画は誰のためのものでしょうか？

* I/O イベントを使用してAdobe CommerceとAdobe Developer App Builderを初めて使用する開発者は、Adobe App Builder プロジェクトを作成する必要があります。

## ビデオコンテンツ {#video-content}

* Commerce管理画面でのAdobe I/O イベントの設定
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
