---
title: Adobe Commerceの設定
description: Adobe Developer App Builderでイベントを使用できるようにAdobe Commerceを設定する方法について説明します。
landing-page-description: Adobe Developer App Builderで使用するイベントメカニズムを使用するようにAdobe Commerceを設定する方法について説明します。
short-description: Adobe Developer App Builderで使用するイベントメカニズムを使用するようにAdobe Commerceを設定する方法について説明します。
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Adobe Commerceの設定

イベントを公開するようにAdobe Commerceを設定する方法を説明します。 追加ドキュメントについては、[Adobe Commerce用のAdobe I/O Eventsのインストール &#x200B;](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"} を参照してください。

## このビデオの目的は誰ですか。

* I/O イベントを使用してAdobe CommerceとAdobe Developer App Builderを初めて使用し、Adobe App Builder プロジェクトを作成する必要がある開発者。

## ビデオコンテンツ {#video-content}

* Commerce admin でのAdobe I/O イベントの設定
* Commerce admin での秘密鍵の保存
* Commerce admin への一意の ID の保存
* イベントプロバイダーの作成

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## 便利なコマンド {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
